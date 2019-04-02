# 3D Convolutional Neural Network Based on Face Anti-Spoofing

标签： `anti-spoofing`

---

论文出处：2017 2nd International Conference on Multimedia and Image Processing  

## 本文提出的方法  


本文提出的是一种基于C3D进行特征提取，这样能够获得不仅仅空间上的特征，并且还能获得时间上的特征。最后把特征放入svm进行分类。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190402212634358.png)


对于数据几乎没有进行预处理，仅仅是做了一个先抽取若干帧作为3DCNN的输入，并且resize到64*64，为了数据增强，在64*64上面随机裁剪57*57大小的图片，最后发现5帧效果最好。觉得本篇文章主要就在于在网络方面进行了一些trick，而对于数据似乎并没做太大的改变。本文参考的[8][20]这两个文章的网络，可以看一下。除此之外对于CASIA进行了交叉验证参考的是[7]的协议，就是把训练集分为5个子部分，然后用其中一个做验证，另外4个训练  

## 收获

1、一种3DCNN的解决方案（可以涉及到时间和空间两个维度）
Face anti-spoofing is becoming more and more 
important in the reliability of face authentication. It can be 
regarded as a two-class problem, that is, the classification of 
real face and forged face.   

## 参考文献重点摘录可作为以后读  

C3D
[8] D.Tran, L.Bourdev, R.Fergus, L.Torresani and M. Paluri, “Learning 
Spatiotemporal Features with 3D Convolutional Networks,” ICCV 
2015.
[20] A. Karpathy, G. Toderici, S. Shetty, T. Leung, R. Sukthankar, and L. 
Fei-Fei, “Large-scale video classification with convolutional neural 
networks,” In CVPR, 2014, pp. 1-6  

数据集处理
[7] J. Yang, Z. Lei, and S. Z. Li, “Learn convolutional neural network for 
face anti-spoofing,” arXiv preprint arXiv:1408.5601,2014. 2, 4.




