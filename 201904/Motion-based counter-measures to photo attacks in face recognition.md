# Motion-based counter-measures to photo attacks in face recognition

标签： `anti-spoofing` `论文`

---

论文出处：IET Biom., 2014, Vol. 3, Iss. 3, pp. 147–158 

## 本文提出的一个数据库  

由于那个时候可用的开源数据库比较少，所以本文提出了一个PHOTO-ATTACK database，主要是照片攻击的数据，里面有50个实体，而每个人的记录都分为Controlled和Adverse，每个视频大概15s也就是有375帧，并且使用MCT[18]进行人脸检测。对于伪造的样本，采用Print，Mobil，Highdef。  

![image](https://ws2.sinaimg.cn/large/005Dd0fOly1g23lis1l7bj30fe07mtbg.jpg)

## 本文提出的方法以及对比方法  

本文对照了三个方法，在文章中位RS1，RS2，RS3，其中RS1[17]，主要利用照片攻击的一个缺陷，就是照片中人的头部只能垂直或者水平移动，所以它就采用比较面部中心和耳朵的移动方向来确定是否为欺诈。RS2[16]提出的是找到一个平面区域，由于照片攻击一般是平面的，所以在OF域里面找到一个平面物体，并且定义一个运动分量，如果运动分量高，则说明为真可能性大。相对于RS1其并不追踪特定的面部。在RS3[13]主要是提取运动的强度，使用grey-scaled frame-difference averages，而本文提出的方法也是类似于RS3，主要是分析头部和背景信息，OFC的步骤是，先进行特征提取，首先计算运动角度，然后计算直方图，计算一个距离，最后用窗口覆盖距离。如下图所示：    

![image](https://wx4.sinaimg.cn/large/005Dd0fOly1g23ljcw59vj30fe02ymxm.jpg)

后面的实验，主要和RS1，RS2，RS3进行比较分析，并且得出最佳结果。  

## 收获  

1、使用光流法可能有效
2、对于打印攻击，多关注其背景和头部的相关性  


## 参考文献重点摘录可作为以后读  

RS1 2 3
13 Anjos, A., Marcel, S.: ‘Counter-measures to photo attacks in face
recognition: a public database and a baseline’. Int. Joint Conf.
Biometrics (IJCB), 2011
16 Bao, W., Li, H., Li, N., Jiang, W.: ‘A liveness detection method for face
recognition based on optical flflow fifield’. 2009 Int. Conf. Image Analysis
and Signal Processing, 2009, pp. 233–236
17 Kollreider, K., Fronthaler, H., Bigun, J.: ‘Non-intrusive liveness
detection by face images’, Image Vis. Comput., 2009, 27, (3),
pp. 233–244


