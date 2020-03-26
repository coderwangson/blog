# pytorch 计算图以及backward

标签 ： `pytorch`

---

## pytorch 计算图   

`pytorch`是深度学习框架，而深度学习其实本质就是一大堆矩阵乘法，最后用来模拟一个高维拟合函数。无论是`pytorch`还是`tensorflow`都是把这些计算保存到一个计算图里面，其实可以看作一颗树，如果学习过数据结构，对于下面的表示应该不陌生：  

![2019-11-26_090900.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1g9b66clwuyj304z04d74a.jpg)  

其实上面这个就是一个计算图，计算了`y = a*w`,这个过程是框架自己做的，我们需要的就是在代码中写下`y = a*w`，这句话就可以了，其实上面这个过程就叫做前向传播，也是深度学习里面比较简单的一步。我们的深度学习一般是用来拟合的，所以要进行反向传播来更新一些参数，最终达到拟合，而这个过程就是反向传播，反向传播的本质是通过梯度下降来进行的，就是计算一些梯度啦，导数啦之类的，不过框架也帮我们实现好了，这个就是`backward()`函数。   

## backward  

backward本身用起来不是很难，但是在一些复杂的网络里面，比如`Gan`这种，可能会有一些小问题，如果对原理不了解，则容易导致报错等。  

下面给一个小的例子，比如我们待拟合的函数是 `y = 5*x`,（假设你对深度学习有基础，懂损失函数之类的，没有的话建议看完再继续），则我们需要一堆的x以及对应labbel，比如(x,label) = (1,5) (2,10) (3,15),然后我们构造一个前向传播 `y' = w *x`，其中这个里面`w`就是我们最终要求的，也就是5，所以需要对w进行更新。简单代码如下 :  

```
import torch
from torch.autograd import Variable
x = torch.Tensor([2])
y = torch.Tensor([10])
loss = torch.nn.MSELoss()
w = Variable(torch.randn(1),requires_grad=True)
print(w)
for i in range(10):
    y_ = w*x
    l = loss(y,y_)
    l.backward()
    print(w.grad.data)
    print(-2*x*(y-y_))
```  

首先，我们定义了一个输入和输出也就是x和y，x为2，y为10，之后我们定义了一个均方误差loss，下面定义了一个`w`,注意`w`是一个Variable形式，并且`requires_grad=True`,为什么需要这样，因为`w`是一个待求的，所以需要进行梯度下降，所以包装成一个`requires_grad=True`的Variable，这样pytorch才能认识，并且w我们是随机生成的，类似于之前学习两层的神经网络时候的w，只不过这个比较简单。 

之后我么就进行了10次迭代，计算了`loss`并进行反向传播，然后打印出来，`print(-2*x*(y-y_))`是我手动计算了一下l对w的导数，然后输出了出来。最后输出如下(部分)：  

```
tensor([-0.8820], requires_grad=True)
tensor([-47.0557])
tensor([-47.0557], grad_fn=<MulBackward0>)
tensor([-94.1115])
tensor([-47.0557], grad_fn=<MulBackward0>)
tensor([-141.1672])
tensor([-47.0557], grad_fn=<MulBackward0>)
tensor([-188.2230])
```  

可以发现我们的w初始为`-0.8820`,计算出来的导数为`-47.0557` 第2行是backward计算的，第3行是我们自己计算的。但是发现下面的第4、5行和想象的不一样，手动计算的好像没问题还是`-47.0557`，但为什么backward计算的变成了`-94.1115`,似乎是`-47.0557*2`,其实是因为pytorch设计就是这样的，它的梯度默认会保存累加，所以这次的梯度是我们这次的加上上一次的。讲到这里，估计也就明白了为什么我们在进行梯度下降的时候需要用`optimizer.zero_grad()`了。主要就是清空梯度。上面的例子只是计算了梯度，并没有进行梯度更新，下面对for循环里面的代码增加点东西:  

```
    y_ = w*x
    l = loss(y,y_)

    l.backward()
    print(w.grad.data)
    w = Variable(w - 0.1 * w.grad.data,requires_grad=True)
    print(w)
    # w.grad.data = torch.Tensor([0])
    print(-2*x*(y-y_))
```  


核心就是增加了`w = Variable(w - 0.1 * w.grad.data,requires_grad=True)`这个代码，熟悉深度学习的应该不陌生，其实就是更新了w，0.1相当于学习率。最后输出为：  

``` 
tensor([-2.0593], requires_grad=True)
tensor([-56.4745])
tensor([3.5881], requires_grad=True)
tensor([-56.4745], grad_fn=<MulBackward0>)
tensor([-11.2949])
tensor([4.7176], requires_grad=True)
tensor([-11.2949], grad_fn=<MulBackward0>)
tensor([-2.2590])
tensor([4.9435], requires_grad=True)
tensor([-2.2590], grad_fn=<MulBackward0>)
tensor([-0.4518])
tensor([4.9887], requires_grad=True)
tensor([-0.4518], grad_fn=<MulBackward0>)
tensor([-0.0904])
tensor([4.9977], requires_grad=True)
tensor([-0.0904], grad_fn=<MulBackward0>)
tensor([-0.0181])
tensor([4.9995], requires_grad=True)
tensor([-0.0181], grad_fn=<MulBackward0>)
tensor([-0.0036])
tensor([4.9999], requires_grad=True)
tensor([-0.0036], grad_fn=<MulBackward0>)
tensor([-0.0007])
tensor([5.0000], requires_grad=True)
tensor([-0.0007], grad_fn=<MulBackward0>)
tensor([-0.0001])
tensor([5.0000], requires_grad=True)
tensor([-0.0001], grad_fn=<MulBackward0>)
tensor([-3.0518e-05])
tensor([5.0000], requires_grad=True)
tensor([-3.0518e-05], grad_fn=<MulBackward0>)

```  

很明显看出最后w更新为5了，和我们的函数拟合上了。这里你可能会问这个也没有做梯度置为0的操作啊，之所以没做是因为我们w重新赋了值，所以里面的w.grad.data变为None了，所以不用重新置为0了。  

说在最后，其实上面这个例子过于简单，所以可能看完还是和你用的`nn.Linear`这种操作无法联系到一起，其实我们的w就相当于`nn.Linear`中的w，只不过pytorch封装起来了，你看不到，而x就相当于你输入的数据，比如mnist的1*28*28的那个矩阵。
## 核心点  

1、 待优化的变量一定要用Variable包装起来，并且requires_grad=True。
2、 为什么需要`optimizer.zero_grad()`







