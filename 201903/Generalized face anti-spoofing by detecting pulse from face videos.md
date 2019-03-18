# Generalized face anti-spoofing by detecting pulse from face videos

标签： `anti-spoofing`

---

文章来源： 2016 23rd International Conference on Pattern Recognition (ICPR)
Cancún Center, Cancún, México, December 4-8, 2016   

## 摘要  

提出当前使用的基于纹理特征活体检测方案存在的不足，提出了本文的方案-基于心率脉冲的方案。并且给出了两个评估数据库：3DMAD，REAL-F。都是主要用3D面具进行欺骗的。

## 引言  

给出了当前主流方案[5][6][7][12]，尤其是LBP以及这些基于纹理的方案，虽然这些方案能在3DMAD这种低质量的数据库中较好表现，但是在REAL-F中却表现平平，或者是不能有很好的鲁棒性，对于未知的新的攻击表型很差。本文给出了基于PPG的方法，通过PPG能够从视频中获取心脏脉冲，而心脏脉冲可以作为是否为活体的检测标准。  


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192721703.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)


## 方法  

提取脸部的下半部[17][18][19]最为ROI->提取每帧三个通道的像素点->把数据预处理->进行FFT变换->计算出六个特征。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192736867.png)

分类器：SVM  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192747824.png)


## 实验  

主要用本文的方案和使用LBP的方案作比较，使用相同的分类器以及参数(这样才说明本文特征有效)，对比标准使用HTER和EER。
在3DMAD上本文的方案不如传统，原因是因为3DMAD数据库假样本质量不佳，所以纹理特征能通过纹理判别。在REAL-F中本文的方案明显优于传统方案，因为REAL-F中的数据假样本比较逼真，所以纹理特征也很像。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192803838.png)


在MSFD中，因为MSFD中的攻击是通过打印攻击或者重放，不同于上面的3D面具，发现在打印攻击上有较好表现，但是对于视频重放攻击表现不佳，因为打印攻击的话PPG测不出心率，而在分辨率高的情况下视频重放能够保留那些信息。所以最后文中提出了一种级联结构，把本文的方案和传统方案级联，最终表现良好，并且具有较强鲁棒性。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192815368.png)  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318192831748.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)



之所以不用当前主流的两个库CASIA和Replay-Attack是因为一个内容不符合，一个分辨率低。  

## 收获

1、了解两个数据库3DMAD[11]以及REAL-F
2、学习了一种新的面部活体特征-PPG以及提取方法
3、对传统基于LBP纹理特征了解
4、两种评比标准EER(equal error rate)和HTER(half total error rate)
5、在低精度的3D模型下可能使用传统纹理特征能有很好的表达，但是在高精度的3D模型下使用ppg方法好
6、对于打印攻击使用ppg方法好，对于重放攻击，则使用纹理特征好  

##  参考文献重点摘录可作为以后读  

讲RPPG的
[15] M.-Z. Poh, D. J. McDuff, and R. W. Picard, “Advancements in noncontact, multiparameter physiological measurements using a webcam,”
IEEE Trans. on Biomedical Engineering, 2011. 
[16]X. Li, J. Chen, G. Zhao, and M. Pietikainen, “Remote heart rate
measurement from face videos under realistic situations,” in Computer
Vision and Pattern Recognition (CVPR), 2014, pp. 4264–4271.    

15年做活体的state of art 
[4]D. Wen, H. Han, and A. Jain, “Face spoof detection with image distortion analysis,” Information Forensics and Security, IEEE Transactions
on, vol. 10, no. 4, pp. 746–761, April 2015   

CASIA和Replay-attack库，我的研究估计会用，可以读一下了解
[24]I. Chingovska, A. Anjos, and S. Marcel, “On the effectiveness of local
binary patterns in face anti-spoofing,” in International Conference of the
Biometrics Special Interest Group (BIOSIG), 2012, pp. 1–7.
[25] Z. Zhang, J. Yan, S. Liu, Z. Lei, D. Yi, and S. Z. Li, “A face antispoofing database with diverse attacks,” in 2012 5th IAPR International
Conference on Biometrics (ICB), March 2012, pp. 26–31.   

基于纹理的活体检测，已经读过
[22]Z. Boulkenafet, J. Komulainen, and A. Hadid, “Face anti-spoofing based
on color texture analysis,” in Image Processing (ICIP), 2015 IEEE
International Conference on, Sept 2015, pp. 2636–2640. 





