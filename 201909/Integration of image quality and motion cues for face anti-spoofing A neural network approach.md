# Integration of image quality and motion cues for face anti-spoofing A neural network approach

标签： `anti-spoofing`

---

## 本文提出的方法  

本文是基于多种特征融合起来的方法，在空间维度上使用SBIQF，并且通过实验说明在对于图片质量评比上要优于LBP，在时间维度使用了光流特征，并且使用了平均光流，在光流方面使用了面部光流和带背景的光流。最终的架构如下：  
![image](http://ws1.sinaimg.cn/large/005Dd0fOgy1g71mb15plfj308307yta8.jpg)

在技术上的点主要是如何提取SBIQF以及光流，这些之前都有人在做相关工作，对于SBIQF可以参考[12]对于光流可以参考[10,11]。在知道了如何计算这些特征之后就是如何处理数据集并且从数据中拿到这些特征。  
本文先把所有视频数据规整成60帧/video,然后从前30帧里面隔5帧取一帧，这样能得到6帧，通过这6帧能得到一个SBIQF和5个面部光流和5个背景光流，然后分别把5个背景光流和5个面部光流取平均，所以得到了3帧特征（一个SBIQF，一个面部平均，一个背景平均），因为是30帧每隔5帧，所以能得到5组，所以一个60帧视频得到10组。然后对于一个视频则把得分平均即可。
而关于光流可用性，以及光流分为背景光流和面部光流可以在下面图片得到解释：  

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g71ma2amrnj30fe03wab8.jpg)  

![image](http://ws4.sinaimg.cn/large/005Dd0fOgy1g71maf97n5j30fe04xmyc.jpg)


对于最后多特征融合来说，文章给出了三种方案，第一是特征层面融合即直接把三类特征连接到一起即可，第二是分数层面即把得分喂入一个LR进行学习，第三个是使用中间层特征也就是bootle-neck特征。最终结果是第三种产生了更好的效果，但是第三种我并不是很理解，尤其是文中使用了自编码。  







