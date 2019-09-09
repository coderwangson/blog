# Image-Based Object Spoofifing Detection

标签： `anti-spoofing`

---

## 本文提出的方法
本文使用的是通过更改色彩空间提取可区分特征的方法进行欺诈检测的。整体架构如下：  

![image](http://ws2.sinaimg.cn/large/005Dd0fOgy1g6tj9i45rkj30fe051gmi.jpg)

之所以提出使用LUV以及Ycbcr是因为相比较RGB，二者多了一些亮度方面的信息，这样得到的特征更加具有可区分性：  

![image](http://wx3.sinaimg.cn/large/005Dd0fOgy1g6tj9x89agj308005rjsc.jpg)

实验结果： 

![image](http://wx4.sinaimg.cn/large/005Dd0fOgy1g6tja6u6j4j30eo0gidia.jpg)



这个表可以作为以后查阅，里面写了很多在Replay上的对比方案，比如[6]是YaoJie Liu提出的用深度图监督方案，以及[31]的别的颜色空间方案。  

## 收获  

1、使用不同颜色空间可能达到意想不到的效果  

## 参考文献重点摘录可作为以后读
### 对比方案  

36.Menotti, D., et al.: Deep Representations for iris, face, and fifingerprint
spoofifing detection. IEEE Trans. Inf. Forensics Secur. 10(4), 864–879 (2015).
http://ieeexplore.ieee.org/document/7029061/

31.Li, L., Correia, P.L., Hadid, A.: Face recognition under spoofifing attacks:
countermeasures and research directions. IET Biom. 7(1), 3–14 (2018).
http://digital-library.theiet.org/content/journals/10.1049/iet-bmt.2017.0089

6.Atoum, Y., Liu, Y., Jourabloo, A., Liu, X.: Face anti-spoofifing using patch and
depth-based CNNs. In: 2017 IEEE International Joint Conference on Biometrics
(IJCB), pp. 319–328. IEEE (2017). http://ieeexplore.ieee.org/document/8272713/








