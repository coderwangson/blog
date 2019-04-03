# Learning Spatiotemporal Features with 3D Convolutional Networks

标签： `论文`

---

论文出处：2015 IEEE International Conference on Computer Vision    


## 3DCNN作为特征提取器  

下面这个图片显示了2D卷积，多帧的2D卷积以及3D卷积的区别，其中W，H是宽和高，L代表帧数，可以看出相对于2D的卷积，3D多了一个时间维度上的联系，所以3D卷积可能学习到时间上的特征，这样会有利于进行视频的分类。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019040321110343.png)

下面提出了本文中对网络的设置，包括把视频帧变为128*121,然后切成16帧不重复的，所以喂入的数据格式为3*16*128*171，然后再进行随机裁剪为3*16*112*112，网络包括5个卷积层，5个池化层（交替出现），两个全连接层，并且用softmax损失，卷积个数分别为64 128 256 256 256,在空间上卷积核的大小为3*3因为[37]发现在空间上3*3最好。而对于时间上的核需要测试得出，本文测试也为3。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190403211120133.png)

并且使用这个设计出的3D卷积架构在不同的数据集上做了测试，并且还用做了一个特征提取器，这样可以提取视频的特征，然后放入到一些简单的分类器中即可。都做了Action regognition，Action Similarity Labeling，Scene and Object Recognition等方面测试。  

## 收获  

1、可以使用3D卷积提取视频特征
2、T-SNE可以做高维特征可视化
3、知道了较好的3D卷积核 3*3*3  

## 参考文献重点摘录可作为以后读   

C3D
[15] S. Ji, W. Xu, M. Yang, and K. Yu. 3d convolutional neural networks
for human action recognition. IEEE TPAMI, 35(1):221–231, 2013.
1, 2
[18] A. Karpathy, G. Toderici, S. Shetty, T. Leung, R. Sukthankar, and
L. Fei-Fei. Large-scale video classification with convolutional neural
networks. In CVPR, 2014. 1, 2, 3, 4, 5, 6  

3*3 空间上的卷积核
[37] K. Simonyan and A. Zisserman. Very deep convolutional networks
for large-scale image recognition. In ICLR, 2015. 3, 4   

高维度特征可视化
[43] L. van der Maaten and G. Hinton. Visualizing data using t-sne.
JMLR, 9(2579-2605):85, 2008. 6, 7
[46] M. Zeiler and R. Fergus. Visualizing and understanding convolutional networks. In ECCV, 2014. 5, 6





