# Biometric spoofing detection by a Domain-aware Convolutional Neural Network 

标签： `antispoofing` `论文`

---

论文出处：2016 12th International Conference on Signal-Image Technology & Internet-Based Systems   

## 基于经验信息  

现在很多地方都用CNN网络进行预测，但是有些地方效果很好，但是有些效果不好，因为在CNN上面学习到的东西不可见，并且可能有些东西并不能自助学习到，比如在[13]的一个例子，CNN就学习不到中值滤波这样简单的滤波器，所以CNN网络在有些时候需要加一些限制或者约束，我们根据我们现有的知识，让我们的CNN网络向着我们觉得正确的方向去学习，这也就是基于经验的CNN网络，加入了人为的因素。而本文的思想正是基于此。提出了从图片中学习到residual image，而为了学习到这种信息，所以在损失函数中加了正则项，如下：  
![image](https://ws2.sinaimg.cn/large/005Dd0fOly1g22632s3ixj30920100sn.jpg)  

![image](https://ws1.sinaimg.cn/large/005Dd0fOly1g2263gl41aj308v078ac5.jpg)  

![image](https://wx2.sinaimg.cn/large/005Dd0fOly1g2263qwrhqj309701hglq.jpg)


这样的话就可以引导着CNN进行学习了。
本文的网络结构就是普通CNN结构。在数据处理上面进行了图片旋转90°，所以图片多了4倍，并且还着重提出，不要进行resize，因为会损失掉某些频率域里面的信息。  

## 收获  

1、要掌握合适的经验信息
2、把这些经验信息加入到损失函数中，对CNN进行指导  

## 参考文献重点摘录可作为以后读  

Residual image 
[11] A. Pinto, W. Schwartz, H. Pedrini, and A. Rocha, “Using visual rhythms for detecting video-based facial spoof attacks,” IEEE Transactions on Information Forensics and Security, vol. 10, no. 5, pp. 1025–1038, may 2015.
[19] D. Cozzolino, D. Gragnaniello, and L. Verdoliva, “Image forgery detection through residual-based local descriptors and block-matching,” in IEEE International Conference on Image Processing, october 2014, pp. 5297–5301 Domain-information
[12] Y. Qian, J. Dong, W. Wang, and T. Tan, “Deep learning for steganalysis via convolutional neural networks,” in IS&T/SPIE Electronic Imaging, 2015, pp. 94 090J–94 090J.
[13] B. Bayar and M. Stamm, “A deep learning approach to
universal image manipulation detection using a new convolutional layer,” in ACM Workshop on Information Hiding
and Multimedia Security, 2016, pp. 5–10.






