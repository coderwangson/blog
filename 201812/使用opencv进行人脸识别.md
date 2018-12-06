# 使用opencv进行人脸识别

标签： opencv 人脸识别 python

---

## 前期准备  

学了一阵子图像了，一直没有做个什么好玩的东西，今天就想着做一个人脸识别来玩一下子，但是又没有数据集训练，就四处搜，终于发现了在opencv中自带的有训练好的模型，我们可以直接使用它的模型做一个具有实时的人的前脸的监测，不过局限性就是，你只能用别人写好的模型，所以只能识别人脸，不过在他的模型里还有一些别的比如身体检测，人眼检测等，感兴趣的可以自行下载使用，如果你安装过opencv，那么你电脑上就有这些模型，如果找不到的话可以通过https://github.com/opencv/opencv/tree/master/data/haarcascades这个地址下载。   

## 原理  

emmm，其实流程很简单，首先通过cv读入一个图片，然后灰度化了。其次我们加载上面说的那个模型，最后我们进行识别，识别的结果会是一个矩阵，我们就把这个矩阵画到图片上就ok了。  

## 代码编制  

```python
import cv2 as cv
# 传入一个img，结果是在人脸上面画一个矩形框
def face_detect(img):
    gray = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
    # 加载opencv人脸识别分类器(训练好的模型)
    classifier = cv.CascadeClassifier("D:\ProgramData\Anaconda3\Lib\site-packages\cv2\data\haarcascade_frontalface_default.xml")
    # 一会绘制矩形的时候的颜色
    color = (255,0,0)
    # 开始识别人脸 
    faceRects = classifier.detectMultiScale(gray,scaleFactor=1.2,minNeighbors =3,minSize = (32,32))
    # 检测到脸的个数>0
    if len(faceRects):
        for rect in faceRects:
            x,y,w,h = rect
            cv.rectangle(img,(x,y),(x+h,y+w),color,2)
    cv.imshow("image",img)
# 捕获视频
video = cv.VideoCapture(0)
while True:
    # 从视频里读取帧
    _,img  = video.read()
    # 显示图片+人脸框
    face_detect(img)
    # 24ms一帧
    cv.waitKey(24)   

```  

上面的代码运行后会弹出一个框框，会调用你的电脑摄像头，然后上面会有蓝色框框把你人脸框出来。  

参考：
>https://github.com/vipstone/faceai/blob/master/doc/detectionOpenCV.md




