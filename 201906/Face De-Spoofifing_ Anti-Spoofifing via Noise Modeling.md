# Face De-Spoofifing: Anti-Spoofifing via Noise Modeling

标签：`论文` `spoofing`

---

论文出处：ECCV2018  


##  本文提出的方法  

传统的欺诈检测一般都是直接从图片提取特征或者是直接用CNN+sotfmax进行分类，而本文把欺诈检测问题定义为了新问题，他把一张欺诈图片分为两部分，一部分是真图片，一部分是欺诈噪声，所以原问题就转化为了de-X问题。    

![image](http://wx3.sinaimg.cn/large/005Dd0fOly1g3nz8yrdukj309v04egmr.jpg)


这样就能把原来的类似黑盒的分类转化的更加可见，这样可以看出到底是什么样的噪声导致的图片为假。但是对于de-X问题来讲，一般都需要有一个groundTruth，比如做去模糊，我们会用一张清楚的图片作为groundTruth，然后对这个图片做一点模糊作为样本，但是由于不清楚欺诈噪声是如何产生的，所以无法来找到groundTruth，同样的由于欺诈的类型也不同，所以可能欺诈的模型也不同。  

对欺诈噪声的研究得出了欺诈噪声的两个特性：无处不在性以及重复性。根据这些特性，可以作为神经网络的约束，神经网络结构如下：  

![image](http://ws3.sinaimg.cn/large/005Dd0fOly1g3nz9kul3mj308o04uglz.jpg)


其中输入I是一个照片，N是经过网络得到的噪声，用I-N则得到live face I^（如果I也是live face则N=0）,这个网络命名为DS Net，就是一个自编码器，其中输入I是6通道图片（RGB+HSV），而为了训练这个DS Net，需要用损失函数约束，所以下面提出了5个损失函数，其中两个是VQnet和DQnet的。  
DQNet是使用的[18]（一个团队的）的网络及模型，就是一个欺诈检测器，一张图片I^（一定是一张live face）经过DQNet能够产出一个深度，然后把这个深度与使用pseudo-depth计算出来的D进行相减就可以得到一个损失。这样就能使得I^更加接近live face，也就约束N越来越接近真正的噪声。在训练DSNet的时候，DQNet不更新参数，DQNet作为已经训练好的直接使用。   

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g3nza0rr6nj30d9021q2x.jpg)


VQNet起到类似于Gan的作用，把VQNet和DSNet放在一起，就起到了Gan的作用，前面的DSNet就相当生成器，产生噪声N然后得到I^,而后面的VQ相当于判别器判别生成的是否能骗过它，如果能骗过他，说明I^就足够好，也说明了N足够好。   

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g3nzaimhnoj30bv01dq37.jpg)  

![image](http://ws3.sinaimg.cn/large/005Dd0fOly1g3nzan8qxaj30c401sjrq.jpg)

除此之外还有三个损失是基于先验知识而产生的，一个是幅度损失，如果输入I是live face，那么其对应的噪声模型要是零。  

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g3nzaqsi6bj309n01oq38.jpg)  

0-1map损失，对liveface来说相当于没有噪声，而对与spoof image来说噪声的一个特点是无处不在，所以liveface对应0map，spoof face对应1map。   

![image](http://wx3.sinaimg.cn/large/005Dd0fOly1g3nzaxh013j30ak03dt9x.jpg)

其中F是自编码器中间的特征。CNN对应网络结构中0-1mapNet。

Repetitiveloss这个是针对噪声具有重复性，而重复性体现在图片的高频区域，所以先把转换到傅里叶域，然后计算出高频，对于spoof图片来说，增大其高频，live来说，减少其高频。  

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g3nzb461sej30ay03bwfr.jpg)

最后把这个5个损失按照权重加起来即可。

实验结果：本文实验结果分为了不同的融合策略的结果，以及不同损失函数的结果，以及不用分辨率的结果。    
![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g3nzbgt4lsj30cq038mxy.jpg)


还有与其他的比较以及交叉库的结果。  

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g3nzbom3z9j306j0590ts.jpg)  

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g3nzbr2lmuj3078058aay.jpg)

下面这个图片给出了一些噪声的可视化：  

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g3nzbuncm9j307x05dju2.jpg)

##  收获  

1、有时候也可以摆脱单纯的CNN模型
2、刺激我想到一种模型，用siams孪生网络，每次喂入3张图片，A为本来的输入样本，B和C一个为真一个为假。如果A是真的，我们可以拉近A和B的距离，否则拉近A和C的距离，最后测试的时候，传入一张图片T，然后再传入一张真的一张假的，看相对那个更加近，这样就能判断出来是真还是假。  

## 参考文献重点摘录可作为以后读  

基于噪声模型的：
2. Patel, K., Han, H., Jain, A.K.: Cross-database face antispoofifing with robust feature repre
sentation. In: Chinese Conference on Biometric Recognition, Springer (2016) 
22. Patel, K., Han, H., Jain, A.K.: Secure face unlock: Spoof detection on smartphones. IEEE 
Trans. Inf. Forens. Security 11(10) (2016) 2268–2283 （2区 IF5.824）







