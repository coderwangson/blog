# 当tensorflow遇见flappy

标签： `tensorflow` `python`

---

本文主要目标是用tensorflow实现识别拳头和手掌，然后融合到flappy中，进行控制小鸟的行动。   

## 拳头手掌分类器模型构建  

### 数据集制作   

这里由于想学习一个完整的流程，所以数据集也自己采集了，并且这个项目较小，所以采集起来也不是特别麻烦，我就是直接从摄像头捕捉到手部图片，然后做出手掌和拳头的操作，保存到硬盘的两个文件夹中用来区分。代码如下：   

```python  
"""
Created on 2019/2/9 18:00
@File: collect_data.py
@author: coderwangson
"""
"#codeing=utf-8"
import cv2 as cv

capture = cv.VideoCapture(0)
count = 100
for i in range(count):
    _,img = capture.read()
    img = cv.flip(img,1)
    img = img[50:350,426:640,:]
    cv.imwrite("./bu/"+"1"+str(i)+".jpg",img)
    cv.imshow("1",img)
    print(i)
    cv.waitKey(24)
```   

这里收集了100张图片，并且写入到`/bu/`这个文件夹下面，说明里面存的都是手掌（布），同样的你可以修改为`/shitou/`这样你做出石头的动作，就可以进行石头（拳头）数据的收集。   

### 数据集处理   

其实正规处理要分为很多，你可以进行数据清洗，数据增强等，这里我为了便于训练就把图片转为灰度并且缩小为`64*64`的尺寸。之后我们可以把图片的数据信息存储起来，这样可以在后续训练的时候直接从一个文件读取即可，这里为了省事，我就直接用`pkl`文件存储下来整个数据集信息。  

```python
"""
Created on 2019/2/9 18:06
@File: process_data.py
@author: coderwangson
"""
"#codeing=utf-8"
import os
import cv2 as cv
import numpy as np
import pickle
dirname = "./shitou/"
a = []
for f in os.listdir(dirname):
    img = cv.imread(dirname+f)
    img = cv.resize(img,(64,64))
    img = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    img = np.reshape(img,(64,64,1))
    a.append(img)
a = np.array(a)
with open("shitou.pkl","wb") as f:
    pickle.dump(a,f)
```  

这里就做了一个简单操作，从文件夹下读取所有图片，然后把图片进行缩放处理。最后我们把像素信息存储到一个ndarray中，我们把这个ndarray存储到一个文件`shitou.pkl`中，这样我们以后使用可以直接从这个文件里拿到那个矩阵了。  

除此之外我还写了两个方法，一个是`get_data`，一个是`get_batch`,这两个方法是在训练模型的时候，我们可以直接使用`get_batch`用来得到一个batch_size的数据。  

```python
def get_data():
    shitou = None
    shitou_label = None
    bu = None
    bu_label = None
    with open("shitou.pkl","rb") as f:
        shitou = pickle.load(f)
        shitou_label = np.zeros((shitou.shape[0],1))
    with open("bu.pkl", "rb") as f:
        bu = pickle.load(f)
        bu_label = np.ones((bu.shape[0],1))
    data = np.vstack([shitou,bu])
    label = np.vstack([shitou_label,bu_label])
    label = label.astype(int)
    label = np.eye(2)[label].reshape(data.shape[0],2)
    return data,label  
    
def get_batch(batch_size):
    data,label = get_data()
    p = np.random.permutation(data.shape[0])[0:batch_size]
    return data[p],label[p]
```  

在get_data中主要是从pkl中拿出数据，然后把拳头和手掌的数据放在一起，并且为他们生成一个标签，注意标签使用的是one-hot格式，`label = np.eye(2)[label].reshape(data.shape[0],2)`这句话就是把普通的转为one-hot格式`2`代表有几个类别。而对于get_batch主要就是得到一个batch的数据，其实就是随机从整个数据集中拿size个数据，这里使用`np.random.permutation(data.shape[0])[0:batch_size]`生成了data.shape[0]个随机数，然后[0:batch_size]取出了batch_size个即可。  

### 训练模型   

上面的前期工作做完后，我们数据集就有了，然后就可以使用tensorflow进行训练然后得到模型了，我们使用CNN构建网络模型，这个大家应该很熟悉，如果不熟悉的话可以参考[一步一步实现CNN卷积神经网络使用numpy并对mnist预测][1] 这篇或者自己去网上找一篇怎么构建一个CNN网络类的文章看一下。  

我们搭建好了网络结构并且损失函数都定义完成就可以进行训练了，这里要记得保存模型为了以后预测使用，具体代码可以去我的github上看，模型代码对应Model.py [tensorflow_flappy][2]。  

### 模型的使用  

经过上面的训练我们得到了一个模型，我们以后就可以使用这个模型进行预测了，给进去一张图片，然后就能得出是拳头还是手掌。  

```python
"""
Created on 2019/2/9 19:16
@File:predict.py
@author: coderwangson
"""
"#codeing=utf-8"
import tensorflow as tf
import numpy as np
import cv2 as cv
import time
def predict(img,sess,x_image,y_,keep_drop):
    y = sess.run(y_, feed_dict={x_image:img, keep_drop: 1.0})
    return np.argmax(y)
# 因为sess加载耗时间，所以在程序中使用，这样我们就能避免每次加载耗时间
start = time.clock()
with tf.Session() as sess:
    saver = tf.train.import_meta_graph("./model/hand.meta")
    saver.restore(sess,tf.train.latest_checkpoint("./model"))
    graph = tf.get_default_graph()
    x_image = graph.get_tensor_by_name("Placeholder:0")
    y_ = graph.get_tensor_by_name("add_3:0")
    keep_drop = graph.get_tensor_by_name("Placeholder_2:0")
    # print(time.clock() - start)
    capture = cv.VideoCapture(0)
    while True:
        ha, img = capture.read()
        img = cv.flip(img,1)
        cv.rectangle(img, (426, 50), (640, 350), (170, 170, 0))
        img = img[50:350, 426:640, :]
        img = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
        cv.imshow("img", img)
        img = cv.resize(img, (64, 64)).reshape((1, 64, 64, 1))

        # 0是拳头 1 是手掌
        action = predict(img, sess,x_image,y_,keep_drop)
        if action ==1:
            print("张开")
        elif action==0:
            print("合住")
        cv.waitKey(24)
```  

这里主要就是那个sess获取，还要图的获取是从模型里面得到的，并且通过测试发现这段代码很耗时间，所以我们获取sess就在游戏里面在刚开始的时候就获取一次即可，避免每次调用predict就要加载一下sess。注意的是我们的图片也要进行resize的处理，和我们制作数据集的时候处理类似。  

## 融合到flappy中   

### flappy游戏  

这个我使用的是别人的代码，可以去我上面git上自己去取，我么主要改动的地方在`mainGame while(True)`这个地方，因为这个地方是游戏的开始地方，所以我们只需要把原来的代码进行改动即可，我们游戏原来是进行捕捉你键盘按键的响应，现在我们则修改成根据你的手势然后自动用win32按下按键，这样你就可以减小代码改动。   

### 融合到游戏里面   

主要是要开启一个摄像头，然后我们在游戏的while(True)里面持续捕获所有帧，然后传入到predict中进行预测，如果是拳头，则模拟按下按键，否则则释放按键。  

```python
    while True:
        ha, img = capture.read()
        img = cv2.flip(img,1)
        cv2.rectangle(img, (426, 50), (640, 350), (170, 170, 0))
        img = img[50:350, 426:640, :]
        cv2.imshow("img", img)
        img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
        img = cv2.resize(img, (64, 64)).reshape((1, 64, 64, 1))

        # 0是拳头 1 是手掌
        action = predict(img, sess,x_image,y_,keep_drop)
        if action ==1:
            win32api.keybd_event(38,0,win32con.KEYEVENTF_KEYUP,0) #释放按键
        else:
            win32api.keybd_event(38, 0, 0, 0)
```   


  [1]: https://blog.csdn.net/qq_28888837/article/details/85858861
  [2]: https://github.com/coderwangson/tensorflow_flappy