# Image-Based Object Spoofifing Detection

标签： `论文` `spoofing`

---

论文出处：Springer Nature Switzerland AG 2018

## 本文提出的方法  

本文使用的是通过更改色彩空间提取可区分特征的方法进行欺诈检测的。整体架构如下：  

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g34dhpnl9nj30gm05cdgw.jpg)  


之所以提出使用LUV以及Ycbcr是因为相比较RGB，二者多了一些亮度方面的信息，这样得到的特征更加具有可区分性：  

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g34di8gzv2j308j06475g.jpg)  

## 实验结果  

![image](http://ws3.sinaimg.cn/large/005Dd0fOly1g34ditlubtj30gj0jln29.jpg)  

![image](http://ws4.sinaimg.cn/large/005Dd0fOly1g34dj59lcvj30eu0bnacq.jpg)  


这个表可以作为以后查阅，里面写了很多在Replay上的对比方案，比如[6]是YaoJie Liu提出的用深度图监督方案，以及[31]的别的颜色空间方案。  

## 收获  

1、使用不同颜色空间可能达到意想不到的效果  


## 参考文献重点摘录可作为以后读  

对比方案  

36.Menotti, D., et al.: Deep Representations for iris, face, and fifingerprint
spoofifing detection. IEEE Trans. Inf. Forensics Secur. 10(4), 864–879 (2015).
http://ieeexplore.ieee.org/document/7029061/

31.Li, L., Correia, P.L., Hadid, A.: Face recognition under spoofifing attacks:
countermeasures and research directions. IET Biom. 7(1), 3–14 (2018).
http://digital-library.theiet.org/content/journals/10.1049/iet-bmt.2017.0089

6.Atoum, Y., Liu, Y., Jourabloo, A., Liu, X.: Face anti-spoofifing using patch and
depth-based CNNs. In: 2017 IEEE International Joint Conference on Biometrics
(IJCB), pp. 319–328. IEEE (2017). http://ieeexplore.ieee.org/document/8272713/





