# Two-Stream Convolutional Networks for Action Recognition in Videos

标签： `论文`

---  

## 双流架构  

在行为检测中，不仅仅是要考虑到空间域上的行为，还要考虑到时间域上的行为，所以本文提出了一种双流解决行为检测的方案。由于RGB图像本身包含有空间信息，所以在空间流上面使用RGB作为输入然后经过CNN网络作出预测，而在时间流上则采用光流的作为输入。然后把二者进行融合，可以直接平均融合，或者喂入SVM进行一个小模型训练。  

![image](https://ws3.sinaimg.cn/large/005Dd0fOly1g271cq46arj30fe061dit.jpg)

空间CNN：RGB图片  

时间CNN：主要是光流图片的堆叠，而本文有采用了Optical flflow stacking（stack是为了体现到不同帧之间的序列信息，把连续的L帧产生了的光流图片堆叠成2*L通道的输入）、Trajectory stacking、Bi-directional optical flflow，并且在这些光流上面采用减平均策略，这样可以避免一些全局信息的干扰。  

## 实验细节  

对于CNN结构来说就是常规CNN架构，在空间流和时间流上面的唯一不同就是时间流上面移除了第二个正则化层，这样减少内存消耗。在训练时，一个批次是随机从256个视频中选取了256帧图片。对于空间网络的训练，则把这些图片随机裁剪成224*224，并做一些数据增强（RGB抖动，随机翻转）。而对于时间网络，对于任意一帧，则产生一个w*h*2L的输入I，其中L代表需要多少个连续序列。比如当前RGB取的是第10帧图片，那么为了取得时间上的信息，则需要向后面在连续取L帧，这样会产生2*L个光流（2代表x方向和y方向各一个）。具体是按照如下公式（sec 3）  

![image](https://ws3.sinaimg.cn/large/005Dd0fOly1g271kwqachj30fe05yq6i.jpg)

这样在时间流的输入就是`256  *（224*224*2*L）`。
训练的时候考虑使用一些ILSVRC的与训练进行微调。对于两个流的结果如下。  

![image](https://wx4.sinaimg.cn/large/005Dd0fOly1g271lrsrnpj30fe048q4j.jpg)

可以看出在空间流上使用微调获得了更好的结果，因为可以避免一些过拟合，并且还测试了[14]提出的一种slow fusion即把RGB堆叠一下。而对于时间流，发现进行堆叠确实获得了不错的结果，在L = 10的时候可能效果更好，并且进行均值减也能产生更好的结果，可能能够减少全局动作的干扰吧。最后的融合采取了直接取平均或者SVM。最终结果如下：    

![image](https://wx3.sinaimg.cn/large/005Dd0fOly1g271m4knf7j30fe0380tx.jpg)




## 收获  

1、如何使用双流结构
2、双流中时间流是如何堆叠的
3、空间流也可以堆叠（参考slow fusion [14]）  

## 参考文献重点摘录可作为以后读  

空间流堆叠  

[14] A. Karpathy, G. Toderici, S. Shetty, T. Leung, R. Sukthankar, and L. Fei-Fei. Large-scale video classication with convolutional neural networks. In Proc. CVPR, 2014.







