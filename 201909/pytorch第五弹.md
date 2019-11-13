# pytorch第五弹

标签： `pytorch` `深度学习`

---

前面第四节讲了关于如何定义pytorch的输入的问题，学会了简单的数据输入，下面就可以直接进行卷积神经网络了，也就是深度学习里面重要的一环，网络结构的搭建--构建前向传播的过程。  

## 卷积操作  

深度学习中最为基础的就是卷积操作，虽然在学习卷积的时候觉得非常难，但是在pytorch里面却定义的非常简单：  

```python
import torch
import torch.nn as nn
import torch.nn.functional as F 
x = torch.rand(1, 3, 224, 224)
conv1 = nn.Conv2d(3, 6, 5)
result = conv1(x)
``` 

很多前向传播的操作都定义在`torch.nn`或者`torch.nn.functional`中，所以可以去这两个模块里面找到你想要的操作。上面就定义了一个简单的卷积操作，其中卷积核的大小为5，输出的特征的通道数为6，输入的特征通道数为3，  Conv2d里面的参数意义，可以参考下面的这个声明一一对应，和之前学习的理论知识中的都能都对应起来。

```
def __init__(self, in_channels, out_channels, kernel_size, stride=1,
             padding=0, dilation=1, groups=1, bias=True):
```  

##  激活函数  

激活函数能够增加网络的非线性，并且也能把数据规范到指定的范围。在torch中激活函数可以在`torch.nn.functional`里面找到  

```python
import torch.nn.functional as F 
x = F.relu(x)
```   

上面的代码就实现了简单的一个relu激活函数，类似的还有tanh等都可以在functional里面找到。  

## 池化和dropout 

这次就直接给出例子：  


```python  
import torch.nn.functional as F 
import torch.nn as nn
x = F.max_pool2d(x, (2, 2))  
x = F.dropout(x, 0.5, training=True)
```  

可以看出都是可以直接调用函数使用的，不过注意在dropout的时候，有一个training的参数，如果你在训练的时候要设置为True，测试则为False，因为测试的时候其实dropout并不起作用了。  

## 小总结  

从上面可以发现其实定义以及使用都非常简单，这里提一下nn里面的和F里面的一点不同。  

从第一个例子可以看出卷积操作是在nn里面，它的使用方式是先定义一个卷积操作对象，然后调用这个对象，而F里面的比如Relu则都是直接使用，这是因为在卷积的时候，卷积核参数都是需要学习得到的，也就是具有可学习的参数，所以要定义成对象，方便以后更新，而Relu则没有可学习的参数，所以直接一个函数就可以了。






