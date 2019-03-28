# Deep Learning for Face Anti-Spoofing: An End-to-End Approach

标签： `anti-spoofing`

---

论文出处： IEEE 2017  

## 本文提出的方法  


本文提出的是一种端到端的CNN架构，因为以前提出的方法，即使是基于CNN的，但是只是把CNN作为了一种特征提取器，而之后再把特征使用SVM进行分类，而本文提出的方案，是一种端到端的方案，末端接了全连接并且使用softmax进行分类。并且给出了一些网络调节的小技巧比如50RS-30SeC-1E等，这些方法如果将来我要是做基于CNN的则可以参考。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190328203823446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

网络是VGG以及两个基于VGG的延伸网络，而对于数据的预处理，只是先把人脸给提取出来，并没有做其他处理。
除此之外，本文给出了很多种结果比较方法，比如top-1 percent accuracy或者accuracy using threshold-operation等，并且与其他方案做了比较。发现还是有所提升。  

## 收获  

1、一种端到端的结构进行欺诈检测 

金句： The remarkable success of Convolutional Neural Networks
(CNN) [8] in ImageNet [6] competition has attracted a multitude of researchers in the computer vision community to
investigate its potential latent capabilities in attaining such
a high performance.  

## 参考文献重点摘录可作为以后读  

其他CNN方法
[20] D. Gragnaniello, C. Sansone, G. Poggi, and L. Verdoliva, “Biometric
spoofing detection by a domain-aware convolutional neural network,” in
Signal-Image Technology & Internet-Based Systems (SITIS), 2016 12th
International Conference on. IEEE, 2016, pp. 193–198.
[21] A. Alotaibi and A. Mahmood, “Deep face liveness detection based on
nonlinear diffusion using convolution neural network,” Signal, Image
and Video Processing, pp. 1–8, 2016.
[22] ——, “Enhancing computer vision to detect face spoofing attack utilizing a single frame from a replay video attack using deep learning,”
in Optoelectronics and Image Processing (ICOIP), 2016 International
Conference on. IEEE, 2016, pp. 1–5.
[23] J. Yang, Z. Lei, and S. Z. Li, “Learn convolutional neural network for
face anti-spoofing,” arXiv preprint arXiv:1408.5601, 2014.
[24] L. Li, X. Feng, Z. Boulkenafet, Z. Xia, M. Li, and A. Hadid, “An original
face anti-spoofing approach using partial convolutional neural network,”
in Image Processing Theory Tools and Applications (IPTA), 2016 6th
International Conference on. IEEE, 2016, pp. 1–6





