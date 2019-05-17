# Transfer Learning Using ConvolutionalNeural Networks for Face Anti-spoofifing

标签： `论文` `spoofing`

---  

论文出处：Springer International Publishing AG 2017

## 本文提出的方法  

本文其实就是进行了迁移学习，因为迁移学习对于数据集小不能进行大规模训练（会过拟合）会有效果。我觉得可能作者在实现的时候用了别的trick，因为他的效果似乎还不错，但是在文章中也就用了迁移学习这个方法，网络结构是把VGG的最后一层移除，最后两层进行了修改。预处理方面仅仅进行了采样（从视频里面提取帧）和面部识别（使用的openface[35]），迁移学习的话，是固定了前面的三层，训练从第四层开始。网络结构如下：  

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g34dltxpt6j30hs07ndid.jpg)

使用的数据库是Replay-Attack和3DMAD，对于3DMAD只使用了rgb图片，没有使用里面的深度信息。
实验结果：  

![image](http://ws3.sinaimg.cn/large/005Dd0fOly1g34dm9dztlj30i7089di7.jpg)

## 收获  

1、未来可以在自己的网络里面使用迁移学习微调  

## 参考文献重点摘录可作为以后读  

实验结果对比中比较好的两个
17. Feng, L., Po, L.M., Li, Y., Xu, X., Yuan, F., Cheung, T.C.H., Cheung, K.W.: Integration of image quality and motion cues for face anti-spoofifing: a neural network approach. J. Vis. Commun. Image Represent. 38, 451–460 (2016)
20. Menotti, D., Chiachia, G., Pinto, A., Schwartz, W.R., Pedrini, H., Falc˜ao, A.X.,
Rocha, A.: Deep representations for iris, face, and fifingerprint spoofifing detection.
IEEE Trans. Inf. Forensics Secur. 10(4), 864–879 (2015) 高引文章  

本文人脸识别
35. Amos, B., Ludwiczuk, B., Satyanarayanan, M.: Openface: a general-purpose face
recognition library with mobile applications. Technical report, CMU-CS-16-118,
CMU School of Computer Science (2016) 


关于迁移学习介绍
26. Sharif Razavian, A., Azizpour, H., Sullivan, J., Carlsson, S.: CNN features offff-
the-shelf: an astounding baseline for recognition. In: The IEEE Conference on
Computer Vision and Pattern Recognition (CVPR) Workshops, June 2014








