# Discriminative representation combinations for accurate face spoofifing detection 

标签： `anti-spoofing` `论文`

---

论文出处：Pattern Recognition 85 (2019) 220–231   

## 本文提出的方法  


本文的贡献提出了几种特征，第一种表征是先使用SSD进行判断，如果对人脸的判断具有不确定，则使用SPMT用来进行局部特征的表征，并且使用的空间金字塔，这样能保证特征具有多尺度的特点。第二种表征需要双目图片，结合了SPMT和TFBD，最后进行分数层面的融合。  

SPMT+SSD结构：  

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g48r5lmr77j308l036wf8.jpg)

从图中可以看出，首先把一个图片喂入SSD（做目标检测的），然后对检测出来的部分进行置信度分析，对于置信度比较低的，在进行SPMT特征提取并且分类。

SPMT+TFBD结构（这个需要双目照片）：  

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g48r5wrao3j309n069tab.jpg)

## 收获  

1、局部信息+全局信息，前景+背景，高频+低频  

##  参考文献重点摘录可作为以后读  

双流的:
[9] K. Patel, H. Han, A.K. Jain, Cross-database face antispoofifing with robust feature 
representation., in: CCBR, 2016, pp. 611–619. 

SSD:
[16]W. Liu, D. Anguelov, D. Erhan, C. Szegedy, S. Reed, C.-Y. Fu, A.C. Berg, SSD: Single shot multibox detector, ECCV, 2016. 

LBPNet(特殊的一层)：
[44] G.B. de Souza, D.F. da Silva Santos, R.G. Pires, A.N. Marana, J.P. Papa, Deep tex
ture features for robust face spoofifing detection, IEEE Trans. Circuits Syst. II 
Express Briefs 64 (12) (2017) 1397–1401. 

在纸质论文中可以看到还有好几个3D的-[15][41] 

欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g48r27k7trj307607674r.jpg)





