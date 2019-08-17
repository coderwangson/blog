# pytorch第五弹

标签： `深度学习` `pytorch`

---

上面说了关于pytorch的输入方案，但是有了输入还要学会对输入进行处理，也就是将输入进行前向传播。在前向传播里面一般包括卷积层，池化层，激活函数等。这些在pytorch中都有对应的实现。  

## 卷积操作  

```
import torch.nn as nn
import torch.nn.functional as F
conv1 = nn.Conv2d(1, 6, 5)
```  

上面就非常简单的定义了一个卷积操作，其中第一个参数1代表输入的数据的通道数，第二个参数代表经过特征图你想要输出的通道数，5代表卷积核的大小。只要你知道什么是卷积操作，然后使用这个工具包就可以直接构造一个卷积核。但是运行你也看不到结果，所以可以通过构造一个随机数作为输入，然后经过这个卷积核，我们来看看效果：  

```python
import torch.nn as nn
import torch
import torch.nn.functional as F
x = torch.rand(1,1,5,5)
conv1 = nn.Conv2d(1, 2, 3)
y = conv1(x)
print(y.shape)
```  

上面则是定义一个随机的输入x，x的维度为`1,1,5,5`分别代表batch大小，通道数（要和Conv2d中第一个参数一致），图片的长和宽。最后会输出一个y。y就是经过卷积操作后的结果，你会发现y的通道变为2,尺寸变为3*3，这是经过卷积后的大小。  

## 激活函数  

在深度学习中有非常多的激活函数，这里以Relu为例子，可以通过下面的操作进行激活函数的使用。   

```  
import torch.nn as nn
import torch
import torch.nn.functional as F
x = torch.Tensor([-1,2,-4,3])
print(x)
x = F.relu(x)
print(x)
```  
先声明了x，然后经过relu函数，发现输出为`[0,2,0,3]`，刚好符合Relu函数的操作。  

## 定义一个简单的网络  

在深度学习里面还有很多常见参数，不过大多数都在`torch.nn`或者`torch.nn.functional`里面，这里不一一展开，当我们有了上面的基本操作之后，就可以学着搭建一个简单的深度网络了。如果你想搭建一个网络，要先继承`nn.Module`，这个和我们之前学的构造一个输入非常类似，然后重写一些方法即可。在这里主要重写`__init__` 和`forward`,其中init是类的初始化，一般在里面定义一些网络的操作，比如你有卷积，池化，激活函数，则把这些操作先定义到init里面，forward里面写前向传播的逻辑，forward里面会有一个参数代表输入，我们则利用init里面定义好的操作来对输入进行运算。   

```
class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(3, 6, 5)
        self.conv2 = nn.Conv2d(6, 16, 5)
        self.fc1 = nn.Linear(256, 120)
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
        x = F.max_pool2d(F.relu(self.conv2(x)), 2)
        x = x.view(x.size(0), -1)

        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)

        return x
```  

上面就是一个简单的网络，从forward里面可以看出，先对输入进行卷积，然后激活函数，最大池化，..,做特征提取，之后把x进行拉伸为（batch，-1）这样的形状，做全连接，最后返回x。  

这里有一个小的python技巧，可能如果python学的不好会觉得上面的代码有点不容易懂，上面定义了self.conv1是一个对象，这个毋庸置疑，但是下面我们使用的时候是直接self.conv1(x)，在其他语言里面没有这样用的，但是python里面有魔法函数，这个就是调用了python中的魔法函数`__call__`,，你可以自行查看这个魔法函数的使用，就明白了。  

定义完网络结构，就可以使用上面的网络了，这里同样随机生成x，然后调用。  
```
import torch
x = torch.randn(5,3,28,28)
net = Net()
y = net(x)
print(y.shape)
```
这里能直接net(x)也是因为上面魔法函数的原因，魔法函数里面自动调用了forward函数。最终输入y的shape为5*10 ，5为batch大小，也就是x中的5，10是最后的分类数，也就是网络中经过最后一个全连接的输出大小`self.fc3 = nn.Linear(84, 10)` .




