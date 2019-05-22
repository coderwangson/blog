# Deep Representations for Iris, Face, and Fingerprint Spoofifing Detection

标签： `论文` `spoofing`

---

论文出处：IEEE TRANSACTIONS ON INFORMATION FORENSICS AND SECURITY  


## 本文提出的方法  

本文是基于深度网络进行的欺诈检测，不过本文的方案不仅仅体现在对现有的网络结构做修改，关键在于学习一个新的网络结构，这样能最大程度保证网络具有更大的泛化能力，所以本文不仅仅在单独的人脸数据库上做了测试，还在指纹以及虹膜上做了测试，均取得不错的结果。本文的主要核心在于两个优化器一个是. Architecture optimization (AO) ，另外一个是fifilter optimization (FO)具体结构如下。  

![image](http://ws4.sinaimg.cn/large/005Dd0fOly1g3adehe1n3j30b60ay0ui.jpg)

AO主要是通过学习找到最优的网络结果，FO就是学习到最优的过滤器。下面这个图体现了对于AO的一些学习的东西。  


![image](http://ws1.sinaimg.cn/large/005Dd0fOly1g3adeqiihsj309d0a5myr.jpg)![image](http://ws1.sinaimg.cn/large/005Dd0fOly1g3adeqiihsj309d0a5myr.jpg)  


可以看出AO需要学习有几层的网络以及一些过滤器的尺寸等信息。而对于FO来说，是现在两个基本网络clf10-11以及spooofnet上面做了优化。    

![image](http://wx2.sinaimg.cn/large/005Dd0fOly1g3adf05uc2j307a06smy6.jpg)


最后是对数据的处理，包括对视频的采样，人脸的识别等。而评估策略则计算ACC或者HTER（有验证集合的）。
最终AO的结果如下：  

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g3adf71ahhj308s0313yx.jpg)

FO的结果如下（主要是对于clf10以及spoofnet，而AO则采用随机参数）：   

![image](http://wx2.sinaimg.cn/large/005Dd0fOly1g3adfdeoqrj305l05bmxq.jpg)


两者融合如下：  

![image](http://ws4.sinaimg.cn/large/005Dd0fOly1g3adfj3irej306m065wfa.jpg)  


## 收获  

1、有些时候网络结够也可以通过学习来确定  

## 参考文献重点摘录可作为以后读  

[55] A. da Silva Pinto, H. Pedrini, W. Schwartz, and A. Rocha, “Video-based 
face spoofifing detection through visual rhythm analysis,” in Proc. 25th 
Conf. Graph., Patterns Images (SIBGRAPI), 2012, pp. 221–228.
[64] T. de Freitas Pereira, A. Anjos, J. M. De Martino, and S. Marcel, “Can 
face anti-spoofifing countermeasures work in a real world scenario?” in 
Proc. IAPR Int. Conf. Biometrics (ICB), 2013, pp. 1–8. 






