# Learning Generalized Deep Feature Representation
for Face Anti-Spoofing  

标签： `anti-spooing`

---

论文出处：IEEE TRANSACTIONS ON INFORMATION FORENSICS AND SECURITY, VOL. 13, NO. 10, OCTOBER 2018

## 本文提出的方法  

本文提出的是一种基于C3D+domain generalization的方案。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190331105624271.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

使用C3D提取出的特征放入SVM进行分类，对于C3D来说，网络结构在文中有给出，主要是对于Generalization loss的涉及，是利用了domain generalization的方案[54]，这样能够增加模型的泛化能力，有效避免过拟合并且对于陌生数据能够有较强适应能力。
对于输入的数据进行了两方面的预处理，一个是空间增强，一个是gamma增强。这样能扩大样本。
最后取得了不错的结果。 并且在各个交叉数据集中也能有较好的泛化能力。
不过本文的C3D以及domain generalization并不是很熟悉，以后需要看相关知识。  

## 收获  

1、一种3DCNN的解决方案（可以涉及到时间和空间两个维度）
金句：  In particular, 3D convolutional neural networks
(3D CNN), which have been proved to be efficient for action
recognition task [11], are employed to learn spoofing-specific
information based on typical printed and replay video attacks  

##  参考文献重点摘录可作为以后读  

C3D
[42] J. Gan, S. Li, Y. Zhai, and C. Liu, “3D convolutional neural network
based on face anti-spoofing,” in Proc. 2nd Int. Conf. Multimedia Image
Process. (ICMIP), Mar. 2017, pp. 1–5.
domain generalization
[54] H. Li, S. J. Pan, S. Wang, and A. C. Kot, “Domain generalization with
adversarial feature learning,” in Proc. IEEE Conf. Comput. Vis. Pattern
Recognit. (CVPR), 2018
其他传统的比较好的方案
[6] J. Galbally, S. Marcel, and J. Fierrez, “Image quality assessment
for fake biometric detection: Application to iris, fingerprint, and face
recognition,” IEEE Trans. Image Process., vol. 23, no. 2, pp. 710–724,
Feb. 2014.



