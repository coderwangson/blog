# CAM Class Activation Mapping -pytorch

标签： `pytorch` 

---

CAM是类激活图，是在Learning Deep Features for Discriminative Localization 这篇文章中提出的，主要的作用是中间层的特征可视化。通过CAM可以看出来在深度网络中图片的哪一部分能起到作用，这样对于深度网络有更好的解释性。  

## 原理  

传统的深度学习，我们通过卷积卷积再卷积，然后我们把最后一个卷积后的东西拉伸成一条，但是这样会破坏空间信息，所以就有了GAP（全局平均池化）这种方案。  

![image](http://ws4.sinaimg.cn/large/005Dd0fOly1g4s4k469qjj30iv0a40y3.jpg)  

GAP其实就是把最后的特征图，对其中的每一个通道都进行一个平均池化，所以最后的结果向量的长度和特征图通道的个数是对应的，之后我们进行一个全连接分类，通过这样的方法，GAP的结果和最后的分类中间又权重，我们通过权重乘上通道，最后组合起来就是类激活图了。  

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g4s4n2wrzrj30gx07b78l.jpg)   

## 代码实现  

### 步骤：  

通过上面的概念可以发现关键在于获得权重和特征图的乘积。特征图我们可以使用pytorch中的hook这个概念进行获取，权重的话，我们把网络的参数列表拿出来，然后对应的最后一对就是最后一层全连接的weights和bias。至于相乘再相加其实可以转化为矩阵。比如我们设通道为`c_x`,所以类激活图就是`w_1 * c_1 + w_2*c_2 + ...`，其实就转化为了一个`(w_1,w_2,...)*(c_1,c_2,...)^T`的向量乘了，对于`c_x`我们又能展开，我们设`c_xy`对应通道`c_x`中的第`y`个点，所以原来的向量乘转化为了`(w_1,w_2,...)*((c_11,c_12,c_13..),c_21,c_22,...),...)^T`,所以我们发现就是把最后的特征图拉伸即可。最后为了把类激活图乘到原图上面，对类激活图进行resize即可，然后和原图相乘就能在原图中展现出哪里重要了。  


### 代码  

```python
from PIL import Image
from torchvision import models, transforms
from torch.autograd import Variable
from torch.nn import functional as F
import numpy as np
import cv2
import json
LABELS_URL = 'https://s3.amazonaws.com/outcome-blog/imagenet/labels.json' # 下载label
# 使用本地的图片和下载到本地的labels.json文件
LABELS_PATH = "labels.json"
# networks such as googlenet, resnet, densenet already use global average pooling at the end, so CAM could be used directly.
model_id = 2
# 选择使用的网络
if model_id == 1:
 net = models.squeezenet1_1(pretrained=True)
 finalconv_name = 'features' # this is the last conv layer of the network
elif model_id == 2:
 net = models.resnet18(pretrained=True)
 finalconv_name = 'layer4'
elif model_id == 3:
 net = models.densenet161(pretrained=True)
 finalconv_name = 'features'
net.eval()
print(net)
# 获取特定层的feature map
# hook the feature extractor
features_blobs = []

def hook_feature(module, input, output): # input是注册层的输入 output是注册层的输出
    print("hook input",input[0].shape)
    features_blobs.append(output.data.cpu().numpy())
# 对layer4层注册，把layer4层的输出append到features里面
net._modules.get(finalconv_name).register_forward_hook(hook_feature) # 注册到finalconv_name,如果执行net()的时候，
                                                            # 会对注册的钩子也执行，这里就是执行了 layer4()
print(net._modules)
# 得到softmax weight,
params = list(net.parameters()) # 将参数变换为列表 按照weights bias 排列 池化无参数
weight_softmax = np.squeeze(params[-2].data.numpy()) # 提取softmax 层的参数 （weights，-1是bias）

# 生成CAM图的函数，完成权重和feature相乘操作，最后resize成上采样
def returnCAM(feature_conv, weight_softmax, class_idx):
    # generate the class activation maps upsample to 256x256
    size_upsample = (224, 224)
    bz, nc, h, w = feature_conv.shape # 获取feature_conv特征的尺寸
    output_cam = []
    #lass_idx为预测分值较大的类别的数字表示的数组，一张图片中有N类物体则数组中N个元素
    for idx in class_idx:
    # weight_softmax中预测为第idx类的参数w乘以feature_map(为了相乘，故reshape了map的形状)

        cam = weight_softmax[idx].dot(feature_conv.reshape((nc, h*w))) # 把原来的相乘再相加转化为矩阵
                                                                    # w1*c1 + w2*c2+ .. -> (w1,w2..) * (c1,c2..)^T -> (w1,w2...)*((c11,c12,c13..),(c21,c22,c23..))
        # 将feature_map的形状reshape回去
        cam = cam.reshape(h, w)
        # 归一化操作（最小的值为0，最大的为1）
        cam = cam - np.min(cam)
        cam_img = cam / np.max(cam)
        # 转换为图片的255的数据
        cam_img = np.uint8(255 * cam_img)
        # resize 图片尺寸与输入图片一致
        output_cam.append(cv2.resize(cam_img, size_upsample))
    return output_cam
# 数据处理，先缩放尺寸到（224*224），再变换数据类型为tensor,最后normalize
normalize = transforms.Normalize(
 mean=[0.485, 0.456, 0.406],
 std=[0.229, 0.224, 0.225]
)
preprocess = transforms.Compose([
 transforms.Resize((224,224)),
 transforms.ToTensor(),
 normalize
])

img_pil = Image.open('test.jpg')

img_tensor = preprocess(img_pil)
# 处理图片为Variable数据
img_variable = Variable(img_tensor.unsqueeze(0))
# 将图片输入网络得到预测类别分值
logit = net(img_variable)

# 使用本地的 LABELS_PATH
with open(LABELS_PATH) as f:
    data = json.load(f).items()
classes = {int(key):value for (key, value) in data}
# 使用softmax打分
h_x = F.softmax(logit, dim=1).data.squeeze() # 分类分值
# 对分类的预测类别分值排序，输出预测值和在列表中的位置
probs, idx = h_x.sort(0, True)
# 转换数据类型
probs = probs.numpy()
idx = idx.numpy()

# 输出与图片尺寸一致的CAM图片
CAMs = returnCAM(features_blobs[0], weight_softmax, [idx[2]])

print('output CAM.jpg for the top1 prediction: %s'%classes[idx[0]])
# 将图片和CAM拼接在一起展示定位结果结果
img = cv2.imread('test.jpg')
img = cv2.resize(img,(224,224))
height, width, _ = img.shape
# 生成热度图
heatmap = cv2.applyColorMap(cv2.resize(CAMs[0],(width, height)), cv2.COLORMAP_JET)
cv2.imwrite('heatmap.jpg', heatmap)
result = heatmap * 0.3 + img * 0.5
cv2.imwrite('CAM.jpg', result)

```  

参考：  

https://zhuanlan.zhihu.com/p/51631163  

http://snappishproductions.com/blog/2018/01/03/class-activation-mapping-in-pytorch.html  

https://github.com/metalbubble/CAM/blob/master/pytorch_CAM.py

欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！  
 
搜索 coderwangson 关注

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g48r27k7trj307607674r.jpg)



