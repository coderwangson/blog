# An Original Face Anti-spoofing Approach using Partial
Convolutional Neural Network

标签： `anti-spoofing`

---

论文出处：978-1-4673-8910-5/16/$31.00 ©2016 IEEE  

## 本文提出的方法  


本文是受[9]的启发，提出了一种DPCNN的结构，先把数据放入一个已经预处理过的VGG中，然后进行训练，之后提取出deep part features，并把这些特征进行PCA降维，最后使用SVM分类。结构如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325203143511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

本文的难点主要在于deep part features的提取，文中给了一些计算的方法，通过第l层的卷积结果，然后经过计算得到应Fx：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325203229628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325203242275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)


最终得到的特征可能维度很大，所以在进行pca的时候分块进行。

实验：本文实验是在CASIA和REPLAY两个库上做的。实验的时候刚开始使用的是第11、13、15、18、20、22、25、27这些层的卷积做deep part features处理，并做了对比：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032520332832.png)

然后把不同层结合起来做了对比：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190325203339146.png)


## 收获  

1、可以使用层中信息做特征  

金句： In recent years, the face biometric technique has been successfully employed in many smart devices and authority systems, such as ATMS, mobile phones and entrance guard systems. With this trend, people even don’t need to remember
the complex password of the credit card, and they can pay
the goods just by scanning their face.  

## 参考文献重点摘录可作为以后读  

本文方案的来源  

[9] R. Girshick, F. Iandola, T. Darrell, and J. Malik, “Deformable part
models are convolutional neural networks,” in IEEE Conference on
Computer Vision and Pattern Recognition, 2015, pp. 437–446. 1, 3（可能对理解那个deep part features有帮助）  

本文用的深度网络 
[24] O. M. Parkhi, A. Vedaldi, and A. Zisserman, “Deep face recognition,”
in British Machine Vision Conference, 2015. 3  

其他CNN方法
[6] J. Yang, Z. Lei, and S. Z. Li, “Learn convolutional neural network for
face anti-spoofing,” Eprint Arxiv, vol. 9218, pp. 373–384, 2014. 1, 2,
3, 6（已看过）
[23] D. Menotti, G. Chiachia, A. Pinto, W. Robson Schwartz, H. Pedrini,
A. Xavier Falcao, and A. Rocha, “Deep representations for iris, face,
and fingerprint spoofing detection,” Information Forensics and Security,
IEEE Transactions on, vol. 10, no. 4, pp. 864–879, 2015. 3




