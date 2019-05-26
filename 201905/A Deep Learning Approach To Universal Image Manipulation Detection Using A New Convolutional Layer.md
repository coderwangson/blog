# A Deep Learning Approach To Universal Image Manipulation Detection Using A New Convolutional Layer

标签： `论文` `spoofing`

---

论文出处：Proceedings of the 4th ACM Workshop on Information Hiding and Multimedia Security  

## 本文提出的方法   

本文做的内容是检测图片是否被修改，之前的方法都是基于不同的修改提出一种方案，这样每次方案设计非常繁琐，设计出一种篡改的特征比较难，所以本文提出了使用CNN网络的方式自动学习到篡改的方法，因为传统的cnn很容易学习到图片的内容，但是不能学习到一些细节，而对于图片是否被篡改的检测来说，图片的内容并不是十分重要，所以本文提出了一个新的CNN卷积层可以作为约束，这个卷积层学习到的是像素以及像素周围的关系特征，而不只是图片内容本身。这个提出的卷积层如下：    

![image](http://wx3.sinaimg.cn/large/005Dd0fOly1g3es7q3v11j30a9057wg1.jpg)


整体的网络结构如下：   

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g3es8b1iy8j30de062wfi.jpg)

通过这个新的卷积层的约束，最终在篡改检测中也取得了不错的效果。   

## 收获  

1、在网络中，可能有时候传统的cnn学习到的东西并不是我们想要的，所以进行一些约束，使得学习的内容接近我们需要的  


## 参考文献重点摘录可作为以后读  

[2] J. Chen, X. Kang, Y. Liu, and Z. J. Wang. Median  fifiltering forensics based on convolutional neural networks. IEEE Signal Processing Letters, 22(11):1849–1853, Nov. 2015.







