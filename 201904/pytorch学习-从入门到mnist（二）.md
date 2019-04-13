# pytorch学习-从入门到mnist（二）

标签： `pytorch`

---  

前面[pytorch学习-从入门到mnist（一）][1]已经说了如何定义网络，如何使用优化器，以及如何进行梯度下降。并且使用回归问题做了测试。这里要使用神经网络最常用的mnist搭建一个简单的CNN进行预测。至于什么是CNN以及卷积层，池化层这里不展开说明。  

## 加载mnist数据集  

pytorch提供了专门的函数来加载mnist，并且进行了专门的处理，会处理成两个pt文件，并且存放在单独的文件夹里面，所以这里需要用pytorch自带的方法进行下载处理，我开始就是把以前的数据集放到根目录下，最后总是出错，发现就是pytroch在处理数据集的时候会先转成pt文件，而这个文件我没有。  
```python

import torch
import torch.nn as nn
from torch.autograd import Variable
import torch.utils.data as Data
import torchvision
import matplotlib.pyplot as plt

EPOCH = 12
BATCH_SIZE = 50
LR = 0.001
DOWNLOAD_MNIST = True # 第一次用True，以后用False
train_set = torchvision.datasets.MNIST(
    root ='./data',
    train = True,
    transform=torchvision.transforms.ToTensor(),# 直接把格式转为tensor
    download=DOWNLOAD_MNIST
) 
# 放到加载器中
train_loader = Data.DataLoader(dataset=train_set,
                               batch_size = BATCH_SIZE,
                               shuffle=True)

```  

## 搭建网络  

```python
# Model
class CNN(torch.nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Sequential(
            nn.Conv2d(in_channels=1,
                      out_channels=16,
                      kernel_size=5,
                      stride=1,
                      padding = 2),
            nn.ReLU(),#(16,28,28)
            nn.MaxPool2d(kernel_size=2)
        ) #(16,14,14)
        self.conv2 = nn.Sequential(
            nn.Conv2d(16,32,5,1,2),
            nn.ReLU(),#(32,14,14)
            nn.MaxPool2d(2)
        )#(32,7,7)
        self.out = nn.Linear(32*7*7,10)
    def forward(self, x):
        x = self.conv1(x)
        x = self.conv2(x)
        x = x.view(x.size(0),-1) # batch_size ,32*7*7
        out = self.out(x)
        return out,x
net = CNN()

```
单纯的卷积，激活，池化，卷积激活池化，最后展平，全连接。   

## 优化器，损失，迭代  

```python
# 优化器
optimizer = torch.optim.Adam(net.parameters(),lr=0.01)
# 交叉熵损失
loss_f = torch.nn.CrossEntropyLoss()
for e in range(EPOCH):
    for step,(bx,by) in enumerate(train_loader):
        predict,last_layer = net(bx)
        optimizer.zero_grad() # 梯度清零
        # 这里交叉熵损失第一个参数一定要是预测的，第二个才是真实的
        # 并且预测的不需要过softmax，因为交叉熵损失内部经过了一次
        # 真实值也不需要进行one-hot，同样因为交叉熵内部帮忙转化了
        loss_v = loss_f(predict,by)
        loss_v.backward() # 反向
        optimizer.step() # 更新
        if step%100 ==0:
            acc = float(torch.sum(torch.max(predict,dim =1)[1]==by))/50.0
            print("epoch {},step {} acc is {}".format(e,step,acc))
```





  [1]: https://blog.csdn.net/qq_28888837/article/details/89277791