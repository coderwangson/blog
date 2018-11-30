## 神经网络中反向传播算法（BP）  

本文只是对BP算法中的一些内容进行一些解释，所以并不是严格的推导，因为我在推导的过程中遇见很多东西，当时不知道为什么要这样，所以本文只是对BP算法中一些东西做点自己的合理性解释，也便于自己理解。  

要想看懂本文，要懂什么是神经网络，对前向传播以及神经网络中一些常见定义要熟悉。  


###  为什么是  $\delta$   

![在这里插入图片描述](https://img-blog.csdn.net/20180929171250286?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

  
 假如上面是一个神经网络的任意层l和l+1层，那么我们如果进行BP算法，就是相当于把一个损失函数$C$ 对$\theta$ 进行偏导数计算，假如现在我要求$\frac{\partial C}{\partial \theta^l}$ ,意思就是对第l层和l+1层之间的权重参数 $\theta$ 求偏导，但是发现密密麻麻好多的权重值呀，所以求其来非常不方便，所以利用链式求导法则，我把这个偏导数转化一下，转化成 $\frac{\partial C}{\partial z^{l+1}}$$\frac{\partial z^{l+1}}{\partial \theta^l}$ ,之所以这样的原因我自己给自己的解释就是为了方便计算，因为$z^{l+1} = \theta^la^l$ 所以求这个$\frac{\partial z^{l+1}}{\partial \theta^l}$ 就相对比较简单 ，所以就把前面的$\frac{\partial C}{\partial z^{l+1}}$ 定义成$\delta^{l+1}$   ,这样的话原问题就转化成了计算$\delta^{l+1}$ $\frac{\partial z^{l+1}}{\partial \theta^l}$，而$\frac{\partial z^{l+1}}{\partial \theta^l}$ 是一个已知的值，所以关键问题就是求 $\delta^{l+1}$ 。 

### $\delta$   的递推  

有了上面的解释，就知道了其实$\delta$的作用就是为了简化计算，现在我们原问题就进行了转换，转换成了求$\delta$，比如你要计算 $\frac{\partial C}{\partial \theta^3}$ 其实关键就是计算出来$\delta^3$ 就行了，所以这个时候你需要求得所有 $\delta$  的值，这样的话你才能对所有的$\theta$ 就行偏导计算，而有定义$\delta^{l} =$ $\frac{\partial C}{\partial z^{l}}$ 并不能看出来什么，但是同样我们用链式法则改变一下你就发现了不一样 ， $\delta^{l} =$ $\frac{\partial C}{\partial z^{l+1}}$  $\frac{\partial z^{l+1}}{\partial z^l }$ 之所以要进行这样的链式，原因还是为了方便，因为直接求偏导很麻烦，所以进行转换，为什么要换成  $\frac{\partial C}{\partial z^{l+1}}$  $\frac{\partial z^{l+1}}{\partial z^l }$ ，是因为 $z^{l+1} = \theta^lsigmoid(z^l)$ ,所以求$\frac{\partial z^{l+1}}{\partial z^l }$  也是能直接求出来的，所以我们就得到了关于$\delta$   的递推  $\delta^{l} =$ $\delta^{l+1}$  $\frac{\partial z^{l+1}}{\partial z^l }$  ,所以整个问题就转化成了如何求$\delta^{l+1}$ ，而对于神经网络来说，也就是如何求最后一层的$\delta$ 。  

至于如何求请看参考中的那篇博文。

### 问题求解  

根据上面的两个解释，就明白了原问题是对$\theta$ 的求偏导，我们把这个用链式法则转化求解$\delta$，而对$\delta$的求导我们同样经过链式法则转化成了求最后一层的那个$\delta$值即可，所以整个问题就转换成了最后一层的$\delta$只要求出来了就行，求出之后，我们根据递推公式计算出所有的$\delta$，然后再根据$\frac{\partial C}{\partial \theta^l}=$  $\delta^{l+1}$ $\frac{\partial z^{l+1}}{\partial \theta^l}$ 就可以求出对所有$\theta$ 的偏导了，因为在经过前向传播后 $\frac{\partial z^{l+1}}{\partial \theta^l}$这个值是可以计算出来的，是一个常数。  

所以你再看BP算法，是不是就有点眉目了，知道为什么要先计算$\delta^L$(L就是最后一层 )， 以及为什么有了$\delta^L$后其他层的$\delta$ 也能计算出来了，以及为什么有了所有的  $\delta$后就能计算所有 $\theta$的偏导了。

![在这里插入图片描述](https://img-blog.csdn.net/20180929174902499?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
  
### 总结    

本文只是从算法角度宏观分析了一下BP算法中每一步这样做的原因，通过解释可以加深对这个算法的理解，关于这个算法的详细推导参考下面参考中的一篇博文，这篇文章给出了推导方法以及最后一层的$\delta$ 的计算以及$\frac{\partial z^{l+1}}{\partial \theta^l}$ 、$\frac{\partial z^{l+1}}{\partial z^l}$  等值的计算公式，这几个偏导都是常规计算，因为有了$z^{l+1} = \theta^{l}a^l=\theta^{l} sigmoid(z^l)$ ,所以偏导数都是可以直接求的。   

参考：
> https://blog.csdn.net/weixin_40920228/article/details/80709216

