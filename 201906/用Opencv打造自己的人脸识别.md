# 用Opencv打造自己的人脸识别

标签： `opencv`

---

人脸识别在现在使用的越来越多，所以使用opencv构造一个简单的人脸识别。步骤包括收集及处理数据，构建人脸识别器，进行人脸识别。  

## 收集数据   

收集数据可以直接使用摄像头或者录制好的视频，从视频中把人脸给取出并保存。取出人脸可以用opencv中自带的一个级联人脸检测。代码如下：  

```python
import cv2
import os
import numpy as np
def generate():
    face_cascade = cv2.CascadeClassifier('./haarcascade_frontalface_default.xml')
    file_name = "2.avi"
    video = cv2.VideoCapture(file_name)
    count = 0
    while True:
        ret,frame = video.read()

        gray = cv2.cvtColor(frame,cv2.COLOR_BGR2GRAY)
        faces = face_cascade.detectMultiScale(gray, 1.3, 5)
        for (x,y,w,h) in faces:
            f = cv2.resize(gray[y:y+h,x:x+w],(200,200))
            dirs = "./data/%s"%file_name
            if not os.path.exists(dirs):
                os.makedirs(dirs)
            cv2.imwrite("%s/%s.pgm"%(dirs,str(count)),f)
            count+=1
        if count ==40:break
        
```  

刚开始的`face_cascade`就是一个人脸检测器，通过这个检测器能够很好的检测出来一个人脸，下面`cv2.VideoCapture(file_name)`就是从一个视频里面读一帧帧的照片，然后把照片转为灰度图，从人脸检测器里面拿到人脸区域，最后保存到data下面，这样就收集好了一个人脸，用同样的方法可以收集多个人脸数据集。  

## 构建人脸识别   

### 读取数据集  

读取数据集的意思是读取你前面保存的人脸数据，我们读取这些数据后，可以利用一些算法构建一个数学模型用来做人脸识别。  

```python
def read_images(path,sz = None):
    c = 0
    X,y = [],[]
    for dirname,dirnames,filenames in os.walk(path):
        for subdirname in dirnames:
            subject_path = os.path.join(dirname,subdirname)
            for filename in os.listdir(subject_path):
                file_path = os.path.join(subject_path,filename)
                im = cv2.imread(file_path,cv2.IMREAD_GRAYSCALE)
                X.append(np.asarray(im,dtype = np.uint8))
                y.append(c)
            c+=1
    return np.asarray(X),np.asarray(y,dtype=np.int32)
```  

这个代码相对比较简单，就是遍历目录，然后把图片数据存到X里面，把其对应的标签存到y里面，这样到时候可以用算法建模。  


### 构建人脸识别  

```python  

def face_rec():
    names = ['No.1','No.2']
    X,y = read_images('./data')
    # 如果没有face.LBPHFaceRecognizer_create 考虑安装opencv-contrib 把以前的卸载了
    model = cv2.face.LBPHFaceRecognizer_create()
    model.train(X,y)
    video = cv2.VideoCapture("1.avi")
    face_cascade = cv2.CascadeClassifier('./haarcascade_frontalface_default.xml')
    while True:
        ret,img = video.read()
        faces = face_cascade.detectMultiScale(img,1.3,5)
        for (x,y,w,h) in faces:
            img = cv2.rectangle(img,(x,y),(x+w,y+h),(255,0,0),2)
            gray = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
            roi = gray[y:y+h,x:x+w]
            try:
                roi = cv2.resize(roi,(200,200))
                predict = model.predict(roi)
                print("Label:%s,Confidence:%f"%(predict[0],predict[1]))
                cv2.putText(img,names[predict[0]],(x,y-20),cv2.FONT_HERSHEY_SIMPLEX,1,255,2)
            except:
                continue
            cv2.imshow("camer",img)
            cv2.waitKey(24)

```  

qianmia，中间的`model = cv2.face.LBPHFaceRecognizer_create()
    model.train(X,y)`就是训练一个LBP算法的识别器，之后我们可以把人脸读取，喂入到这个模型，模型会返回一个预测结果，我们根据这个预测结果就能得到这个人是谁。  
    
结果如下：   

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g452u7m4kjj309t0c9tcm.jpg)  


欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！
![微信公众号](https://mmbiz.qpic.cn/mmbiz_jpg/jHLoMzblJGib3edEia7P3RicYib1HqcK5ItwKCibTW89mgx6KIbpgqQ2hJlWWbLuMhiclKZvjg1GD10HqIktoKEPo18g/0?wx_fmt=jpeg)







