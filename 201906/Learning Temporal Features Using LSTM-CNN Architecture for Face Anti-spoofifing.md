# Learning Temporal Features Using LSTM-CNN Architecture for Face Anti-spoofifing 

标签： `论文` `anti-spooing`

---

论文出处： 2015 3rd IAPR Asian Conference on Pattern Recognition    


## 本文提出的方法  


本文的贡献有，使用了端到端的网络进行欺诈检测，提出了LSTM-CNN结构，并且实验得出图片背景信息的有效性。   

本文的CNN结构（两层CNN，文中给出了详细的卷积核情况），输入的是一帧帧的人脸，人脸使用opencv中的Viola-Jones提取：  

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g3xhdw4k8nj307l03aglw.jpg)  


LSTM-CNN结构，输入是多个经过CNN提取，并且经过FC的特征，因为LSTM可以提取时间信息，所以输入就是一个视频中的经过特征提取的多帧。  
![image](http://wx3.sinaimg.cn/large/005Dd0fOly1g3xhe8hiwpj309009fq3p.jpg)

除此之外，还验证了不同面部占比（可以突出背景信息）的作用（在[19]中也有该方法）。  

![image](http://ws1.sinaimg.cn/large/005Dd0fOly1g3xhegovtaj309603h0tj.jpg)  


## 收获  

1、时间信息可以用LSTM获取  

## 参考文献重点摘录可作为以后读  

视频分类的  

[15] J. Y.-H. Ng, M. Hausknecht, S. Vijayanarasimhan, O. Vinyals, R. Monga, and G. Toderici. Beyond short snippets: Deep networks for video classifification. arXiv preprint arXiv:1503.08909, 2015. 2, 3 





