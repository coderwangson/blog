# CNN Based Spatio-temporal Feature Extraction for Face Anti-spoofing

标签： `anti-spoofing`

---

论文来源： 2017 2nd International Conference on Image, Vision and Computing   

## 摘要  

当前很多方法都只考虑了单帧图像，也就是只考虑了在空间上的特征信息，而忽略了时间轴上的信息，所以本文提出了一种CNN（空间信息）+LBPTOP（时间信息）的方法进行特征提取。选用了CASIA和Replay-Attack数据库做实验。

## 引言  

先前的方法都是使用CNN[1]的方法，并且还使用了人脸识别把人脸扣出来，其实其忽略了背景信息，其背景信息也很重要（因为人会相对背景产生动作）。本文的方法不需要对图片预处理（主要指的是进行人脸切割）。除此之外还罗列了其他的几种常见的方法：motion based，texture based，3Dshape based。
这里面提到了一种cnn+lstm可以看一下[1]lstm可以有时间信息

## 背景知识  

提出的结构是CNN+LBP-TOP，其中CNN提取空间信息，LBP-TOP提取时间信息，CNN架构使用的是[16](Hinton提出的ImagNet那个)，LBP-TOP使用[18]提出的VLBP方法，使用VLBP进行计算并产生相应直方图最为最终特征。CNN图片不贴了，就是普通的卷积加池化。下面的图是LBP-TOP的。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192149766.png)


## 方法   

视频数据处理->由于数据集中各个视频帧数不同，所以统一规定选取前60帧，然后都resize成227*227（可以作为参考）
把一个视频（60帧）喂入CNN计算特征，取CNN的第3、4、5层特征（每层有每层的特点）
然后把每层的特征喂入LBP-TOP计算直方图作为特征（有小技巧，参考原文Figure 3）？（这点不太明白，如何作为特征的，以后可以看LBP-TOP[18]）
最后放入SVM训练
分类器：SVM
**注意**：这个喂入CNN的是视频即[60,227,227,3]，如果实现的话要注意，而不是传统的图片[batch_size,w,h,c]  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192224810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

##  实验  

数据集使用的是CASIA[21]和REPLAY-ATTACK[17]，评测标准使用HTER和EER。注意在CASIA中没有交叉验证集合（即validation或者dev集合），所以自行从train中分出来。下图是实验结果。  


![在这里插入图片描述](https://img-blog.csdnimg.cn/2019031819224915.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192255759.png)

下面这个图片是看一下不同CNN层的特征对结果的影响，是自己和自己的实验做对比。  


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192307274.png)

可以看出还是把所有层都用上效果较好，并且层次越高效果也越好，说明CNN越往高层，就能产生更能代表该图片的特征信息，但不是全部，底层也含有一些高层不具备的信息，所以使用所有层次最后效果更好。


## 收获

1、了解了对于活体不仅仅可以做图片方面工作，还可以放到视频方面考虑时间轴信息
2、了解了LBP-TOP用处参考[18]
3、对CASIA以及REPLAY数据集中视频数据如何处理  

金句：In the volume, the author samples the surrounding points and then with the value of center pixel thresholding every point in the neighborhood.   


## 参考文献重点摘录可作为以后读  

LSTM和CNN结合的
[1] z. Xu, S. Li, and W. Deng, "Learning temporal features using LSTMCNN architecture for face anti-spoofing," Proc. - 3rd IAPR Asian
Con! Pattern Recognition, ACPR 2015, pp. 141-145,2016.   

一些其他特征方法
[3]-[12]参考原文Introduction第三段（Traditional methods）
LBP-TOP
[18]G. Zhao and P. Matti, "Dynamic Texture Recognition Using Local
Binary Patterns with an Application to Facial Expressions," Pattern
Anal. Mach. Intel!. Trans., vol. 29, no. 6, pp. 915-928, 2007   

(本文采用的)
[15]T. De Freitas Pereira, A. Anjos, 1. M. De Martino, and S. Marcel,
"LBP-TOP based countermeasure against face spoofing attacks," Lect.
Notes Comput. Sci. (including Subser. Lect. Notes Artif. Intel!. Lect.
Notes Bioinformatics), vol. 7728 LNCS, no. PART I, pp. 121-132,
2013.   


CNN权威模型
[16] A. Krizhevsky, 1. Sutskever, and G. E. Hinton, "lmageNet
Classification with Deep Convolutional Neural Networks," Adv.
Neurallnj Process. Syst., pp. 1-9,2012 





