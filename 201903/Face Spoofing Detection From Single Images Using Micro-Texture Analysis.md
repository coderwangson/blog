# Face Spoofing Detection From Single Images Using Micro-Texture Analysis

标签： `Leetcode`

---

论文出处：978-1-4577-1359-0111/$26.00 ©2011 IEEE   

## 摘要  

因为打印图片在质量上与真正的人脸有差别，所以本文提出了一种基于LBP特征的活体检测。除此之外，本文提出的方法融合了人脸检测+活体检测，并且更加健壮。

## 引言  

主要就是讲了当前人脸作为生物特越来越重要，但是其安全性得不到保障，所以就有了活体检测这个需求。一般的攻击有视频攻击、面具(mask)、3D打印、照片打印等。典型的对策可以使用一些体征，比如眼眨或者面部表情。也有的是多个生物模型混合（比如面部+声音或者步态(gait)）。
本文使用的是图片质量，由于打印图片内在的打印缺陷，以及面部是复杂的非刚性3D模型（non rigid 3D object）但是打印照片只是平面的（planar rigid object），所以提出了使用LBP[9] 进行特征提取然后喂入SVM中。

## 相关工作  

之前的研究有眼动法、光流法、DoG等，但是都具有缺点。所以才有了本文的方案使用LBP，具有快速，并且特征具有高度的可区分性。本文使用的数据库是NUAA[12]。

关于微纹理（Micro-Texture）：其实就是LBP，本文的LBP主要使用的就是[9]中的，其中对于LBP的定义以及等价LBP的定义可以参考本文第三节或者[9]，$LBP^{ux}_{P,R}$，其中R代表在周围R区域内采样，而P代表采样点的个数，u代表使用等价模式，而x代表超过x的跳数公用一个模式。所以本文最终的使用特征是一整张图片的两种LBP，以及把一张图片分为9个区域的LBP所以共833个特征，然后喂入SVM。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190321213209301.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

因为单独的一种LBP，似乎不具有区别性，所以这里给出了多种模式：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032121331088.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  


## 实验  

实验就是先得到833个特征，然后把特征放入SVM分类器即可，计算得到的EER以及ROC曲线如下：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190321213406620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190321213416249.png)


本文还分析了那些出错的图片发现都是具有过度曝光或者模糊的：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032121342226.png)

除此之外还对比了本实验中不同的LBP的组合的结果:  

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032121343862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

从这个结果图可以看出不同LBP组合的情况：  

![a](https://img-blog.csdnimg.cn/20190321213445590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)


## 收获

1、学习了关于LBP的知识
金句：It is a powerful means of texture description and among its properties in real-world applications are its discriminative power, computational simplicity and tol-erance against monotonic gray-scale changes.   


## 参考文献重点摘录可作为以后读
LBP
[9] T. Ojala, M. Pietikainen, and T. M aenpaa. M ultiresolution 
gray-scale and rotation invariant texture classification with 
local binary patterns. IEEE Trans. Pattern Anal. Mach. Intell., 24:971-987, July 2002. 2,3,4 
NUAA数据库
[12] X. Tan, Y. Li, J. Liu, and L. Jiang. Face liveness detection 
from a single image with sparse low rank bilinear discriminative model. In Proceedings of the 11th European conference on Computer vision: Part VI, ECCV'lO, pages 504-
517, Berlin, Heidelberg, 2010. Springer-Verlag. 2, 4, 5, 6 




