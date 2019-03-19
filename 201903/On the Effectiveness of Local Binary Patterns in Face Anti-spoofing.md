# On the Effectiveness of Local Binary Patterns in Face Anti-spoofing

标签： `anti-spoofing`

---

论文出处： 2012 BIOSIG - Proceedings of the International Conference of Biometrics Special Interest Group (BIOSIG)  

## 摘要  

提出当前使用的面部识别系统非常容易被攻破，所以本文给出了一种基于LBP下的对于打印攻击以及视频攻击能有效防止，并且还给出了一个REPLAY
-ATTACK数据库，并且使用LBP方案在上面给出了一个baseline。

## 引言  

当前的很多系统都很容易被攻破，可以参考[2]，只需要一个照片就能攻破一个平板电脑的人脸系统，由于照片大量存在互联网中，所以找到一个人脸的方法也是非常廉价的。但是现在由于没有合适的公共数据库，所以目前anti
Spoofing发展缓慢，所以本文给出了一个公共数据库，并且给出了一种使用LBP的方案在数据库上产生的baseline。  


## 相关工作  

当下的用于活体检测的方案有三个方向： analyzing the texture of the image captured by the sensor, detecting any evidence of liveness on the scene or combining both approaches together.[7]提出了一种基于LBP的方案进行纹理检测。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190319194758772.png)

也还有一些其他的方案，比如基于眼动等。
当前流行的数据库：NUAA，缺点是只有打印攻击，样本个数少，只有静止的照片，并且样本没有分出来交叉验证集（developset）。CASIA-FASD包含了三类攻击，可以看做是在NUAA的增加，但是也是没有分出来交叉验证集。所以这篇文章提出了REPLAY-ATTACK。
RELPAY-ATTACK数据库：里面包含真脸，打印，手机照片，平板照片，并且划分了训练集，验证集以及测试集三部分。每个视频为25f/s*15s=375帧。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190319194852541.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)
	

## 实验  

实验方案来源于[7]和[14]，使用了LBP对图像进行特征提取，在提取特征之前先把视频处理成帧图片，这样可以和NUAA数据库统一，并且先使用人脸提取出来人脸部分处理为64*64的图片。对于LBP3*3的提取也分为两种，第一种是提取整个图片的（有59个特征）,或者先把图片分块（分成9块）则有59*9 = 531个特征，本文实现时使用了BOb[18]库http://www.idiap.ch/software/bob/。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190319194947642.png)

## 实验结果  

关于实验结果的测量指标：EER和HTER和ROC。除此之外本次实验还是用了控制变量，第一先控制使用哪种类型特征，分别使用了基本的LBP和扩展的LBP[17]进行了测试发现其结果都差别不大，并且其他的LBP特征的计算量很大，所以就使用基本的LBP。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190319195008736.png)

第二就是控制是否把图片切割成9个块，发现也没有显性的变化，所以就使用整张图片。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019031919501975.png)  


第三是使用不同的分类器，分别使用了线性分类器LDA和非线性分类器SVM，发现SVM并没有提升过多，所以就选择LDA，这样其性价比比较高。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/201903191950308.png)

在选出来比较好的特征以及分类器后，在不同的数据库上测试了一下，验证一下其泛化能力是否够，并且与[7]的结果进行了对比，结果有出入，文中给出了出入的可能原因。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190319195037326.png)

展望：未来的方向可能是在于对于特定的攻击做特定的模型，也可能是做一个泛化能力比较强的模型。

## 收获

1、了解2012年主流数据库NUAA等及其缺点以及当时的主流方法
2、了解了REPLAY-ATTACK数据库
3、着重了解了LBP在活体检测上的使用  

## 参考文献重点摘录可作为以后读  

本文源代码
Github可以找到 直接搜论文名称  

LBP
[7] J. Maatt a, A. Hadid, and M. Pietik ainen, “Face spoofing detection from single images using micro-texture analysis,” in Proc. International Joint Conference on Biometrics (IJCB 2011), Washington, D.C., USA, 20
[14] T. Ojala, M. Pietikainen, and T. Maenpaa, “Multiresolution gray-scale and rotation invariant texture classification with local binary patterns,”IEEE Tran. Pattern Anal. Mach. Intelll. 24 no. 7 20
[15] J. Trefny and J. Matas, “Extended set of local binary patterns for rapid
object detection,” Computer Vision Winter Workshop, Czech Republic,
2010.  

Bob一个库
[18] A. Anjos et al., “Bob: a free signal processing and machine learning
toolbox for researchers,” in 20th ACM Conference on Multimedia
Systems (ACMMM), Nara, Japan. ACM Press, Oct. 2012
2http://www.idiap.ch/software/bob/
3Code available at: https://github.com/bioidiap/antispoofing.lbp

