# Learning Deep Models for Face Anti-Spoofing: Binary or Auxiliary Supervision

标签（空格分隔）： `anti-spoofing`

---
## 摘要
由于面部识别的广泛使用，所以对于面部识别的反欺诈也尤为重要，而先前的工作都把面部反欺诈作为二分类的问题，但是本文探讨了辅助监督（ auxiliary supervision）的重要性。利用CNN-RNN得到的面部深度（face depth）以及rPPG（利用sequence-wise supervision得到）来进行面部反欺诈。    

##  方案
本文提出了一种深度模型，他使用空间和时间辅助信息的监督而不是二元监督，以便从面部视频中更健壮地检测面部呈现欺诈(PA)。这些辅助信息是基于我们关于现场和欺诈面部之间关键差异的领域知识获得的，其中包括两个视角：空间和时间。其中空间就是图像的深度（face-depth），而时间就是使用rPPG信号作为辅助监督。

关于Spoof in the Wild Database(SiW)- 他是一个数据库，现有的面部反欺骗数据库，如NUAA ，CASIA ，ReplayAttack 和MSU-MFSD ，都是在3 - 5 年前收集的。而现在电子产品的快速发展，与现有技术相比，这些数据收集中使用的设备类型（例如，相机）在分辨率和成像质量方面已经过时。鉴于对更高级数据库的明确需求，他们收集了一个面部反欺骗数据库命名为Spoof in the Wild Database(SiW)。

本文的三个主要贡献：
 1.建议利用新颖的辅助信息（即深度图和rPPG）来监督CNN学习以改进泛化。
 2.提出了一种新颖的CNN-RNN架构，用于端到端学习深度图和rPPG信号。
 3.发布了一个新的数据库，其中包含PIE的变体和其他实际因素。实现了面部反欺骗的最先进性能。
早期反欺骗工作：基于纹理的方法，基于时间以及基于rPPG信号。

本文提出的方法：
基于CNN-RNN架构的深度学习，CNN部分利用深度图像(depth map)监督来识别细微的纹理特征，然后把估计到的深度以及特征图(feature maps)喂入一个novel non-rigid registration layer中并创造出一个新的特征图(feature maps)，而RNN部分则利用上面生成的新的特征图(feature maps)和rPPG进行训练。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190318191909171.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)


## 结论
本文确定了辅助监督对于基于深度模型的面部反欺诈的重要性，并且提出了结合CNN和RNN架构的网络模型。  

## 我自己的总结
本文使用的方法相对之前具有新颖之处，之前可能只是使用rPPG一个方面，而本文使用了结合rPPG以及图像深度两个方面达到更好的效果。





