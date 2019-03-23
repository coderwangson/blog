# Learn Convolutional Neural Network for Face Anti-Spoofing

标签： `anti-spoofing`

---

论文出处：CoRR, abs/1408.5601, 2014.2, 7, 8  

## 摘要  

在之前的关于欺诈检测的大多数方案都是基于手工特征的，本文提出了一种基于CNN的方案，并且能够在交叉库的检测中有较好的表现。

## 引言  

以前的对于欺诈检测的方案有基于纹理的Texture-based Anti-Spoofing:,基于动作的 Motion-based Anti-Spoofing，基于3D模型的 3D Shape-based Anti-Spoofing:，利用亮度等特质的Multi-Spectral Reflectance-based Anti-Spoofing。

## 方案

A.数据处理 ：对面部进行定位，使用的是[28]的方案，然后对数据进行空间上的增强，主要是把脸部的数据扩大，增多背景信息，原因来自[4,35]。还有就是时间域上的增强，一次喂入多帧图片，来源于[11]的结论。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190322200517459.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

B.特征提取：使用的就是一个[21]提出的CNN网络。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190322200533763.png)

C.使用SVM对上面提取的特征进行分类

实验：实验主要是在两个库--CASIA和REPLAY中，并且按照[10]的协议做了交叉库的实验，体现了CNN的泛化能力。并且还对于图片进行空间和时间上的增强做了对比。返现了空间上的增强是有效的并且的出比较好的空间上的参数以及时间上的参数。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190322200545106.png)

并且也分析了为什么在Replay和casia对于空间上的增强有不同的表现。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032220055526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190322200605238.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

## 收获

1、学习了如何对喂入CNN的数据做一下子预处理（空间增强和时间增强）  

金句： As a result,when the scale is too large, genuine and fake samples in
CASIA dataset become more similar rather than different,
whereas they are more discriminative on REPLAY-ATTACK
dataset.  

## 参考文献重点摘录可作为以后读  

本文用的深度网络
[21] A. Krizhevsky, I. Sutskever, and G. E. Hinton. Imagenet classification
with deep convolutional neural networks. In NIPS, pages 1106–1114,
2012.  

基于3D的
[33] T. Wang, J. Yang, Z. Lei, S. Liao, and S. Z. Li. Face liveness detection
using 3d structure recovered from a single camera. In Biometrics
(ICB), 2013 International Conference on, pages 1–6. IEEE, 2013





