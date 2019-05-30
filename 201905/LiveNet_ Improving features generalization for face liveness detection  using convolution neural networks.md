# LiveNet: Improving features generalization for face liveness detection  using convolution neural networks 

标签： `论文` `spoofing`

---

论文出处：Expert Systems With Applications 2018 Elsevier Ltd. All rights reserved.  

## 本文提出的方法  

本文主要提出的是使用深度网络进行欺诈检测，但同时也做了在交叉库上的测试，因为之前的工作都是在单一库上面的测试，这样并不能证明提出来的方法是否具有较高的泛化能力。除此之外本文提出了一种训练方法，这种训练方法可以减低深度网络在小样本上面的过拟合以及减少训练时间。并且本文多次强调他们使用的是端到端的网络。并且也指出之前的方案大多数都不是端到端的，并且其他的也很少进行交叉库测试。
本文的训练方法如下：    

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g3jm633d04j309f04v0sx.jpg)


上面是传统训练方法，每个epoch是先把样本打乱，然后多个minibatch取出所有数据，而下面是本文提出的方案，是每个小epoch是先打乱，然后取一个batch，再次打乱，再次取，一个大的epoch里面有60个小epoch（后面提到，这个小的epoch的大小可以当做超参数）。并且通过实验证明了在一定程度上减小了过拟合。（后文也说到，可能的原因是在一个大epoch中其实并没有把所有的样本都取出，所以可能在后面的大epoch中会遇到新的样本，这样会重新学习，降低了过拟合的可能）。   

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g3jm6dik14j307f05dwf6.jpg)

训练过程：先通过Viola-Jones的面部识别方案识别出面部，然后再进行一些标准化的操作，因为是视频，所以要先降采样，本文方案是每个视频取出来100帧。然后使用上文的训练策略按照epoch训练。
本文的结果-在内部库测试（非交叉）：   

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g3jm6k59mij309j0400t5.jpg)

本文的结果-在内部库测试（交叉库）比如在Casia上训练，在replay上面测试：  

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g3jm6ujqr7j30an04uwf5.jpg)

## 收获  

1、在网络的训练过程也可以考虑加一些策略，我记得之前的一篇论文Deep Learning for Face Anti-Spoofing: An End-to-End Approach 也提到了一种训练策略。   

2、本文可能并没有在单独的库上面有很好的结果，但是主要是做了交叉库，所以也可以考虑设计一个比较泛化的模型   

##  参考文献重点摘录可作为以后读  

**Diffuse Image**

Alotaibi, A., & Mahmood, A. (2017). Deep face liveness detection based on nonlinear diffusion using convolution neural network. Signal, Image and Video Processing, 11(4), 713–720.   

**CNN+RNN**
Xu, Z., Li, S., & Deng, W. (2015). Learning temporal features using lstm-cnn archi tecture for face anti-spoofifing. In Pattern recognition (acpr), 2015 3rd IAPR asian 
conference on (pp. 141–145). IEEE. 








