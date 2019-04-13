# pytorch学习-从入门到mnist（一）

标签： `pytorch`

---  

在学习过`Tensorflow`之后，其实学习pytorch并不是特别难，因为很多地方是相通的，可能每个库会有自己的深层特性，但是在初学的时候对于简单api的调用还是易懂的。

## pytroch定义Tensor  

因为在这些深度学习库中`张量`是一个通用的东西，所以要先学会怎么定义一个`Tensor`,简单的我们可以直接把`ndarray`变为一个Tensor，并且`pytorch`中有很多方法是和`numpy`类似的，比如`linsace  max min `等。   

```python
import torch
import numpy as np 
nd = np.arange(4).reshape(2,2)

# ndarray转为torch可以通过from_numpy
td = torch.from_numpy(nd)
# 也可以直接把放入FloatTensor
td = torch.FloatTensor(nd)
# FloatTensor里面可以直接放一个数组  
tensor = torch.FloatTensor([[1,2],[3,4]])  

# 也可以把一个tensor转换为ndarray  
print(td.numpy())
# torch中有很多和numpy一样的方法
print(torch.min(td));print(torch.sin(td))

# 常见的矩阵运算，和numpy类似，要注意dot  
print(np.matmul(nd,nd))
print(torch.mm(td,td)) # mm相当于matmul缩写  
# 但是注意dot，在np中和matmul类似，但是torch中的是先进行展平，再相乘(toch3以上废除)
print(np.dot(nd,nd))
```

### pytorch中Variable  

在使用的过程中我没有觉得Variable有什么区别，在有些地方说Variable是对Tensor的一层封装，Tensor存在于Variable的data中，Variable构建了一个计算图，但是在搭建CNN的时候觉得似乎没什么不同，所以就先学习一下如何把Variable和Tensor进行转换，以后用到在说。  

``` python
import torch
from torch.autograd import Variable
# 定义一个Tensor
tensor = torch.FloatTensor([[1,2],[3,4]])
variable = Variable(tensor,requires_grad = True)
#把Variable转为tensor
print(variable.data)
# 如果想把一个变量变为numpy，则先变为tensor再用.numpy
print(variable.data.numpy())

# 定义一个函数 1/4（v1^2+v2^2+v3^2+v4^2）
v_cost = torch.mean(variable*variable)
# 求导
v_cost.backward()
# 从求导可以看出对v1各个梯度都为1/2*v_x,
print(variable.grad) 
# 输出为 tensor([[0.5000, 1.0000],[1.5000, 2.0000]])
```  

## 激活函数   

这个就比较简单了，只要知道是在哪个包里面就可以了。  

```python
import torch
import torch.nn.functional as F
from torch.autograd import Variable
import matplotlib.pyplot as plt

x = Variable(torch.linspace(-5,5,100))

y_relu = torch.relu(x).numpy()
y_tanh = torch.tanh(x).numpy()
y_sigmoid = torch.sigmoid(x).numpy()
y_softluse = F.softplus(x).numpy()
x_np = x.data.numpy()

plt.subplot(221)
plt.plot(x_np,y_relu,label="relu")
plt.legend(loc='best')

plt.subplot(222)
plt.plot(x_np,y_tanh,label="tanh")
plt.legend(loc='best')

plt.subplot(223)
plt.plot(x_np,y_sigmoid,label="sigmoid")
plt.legend(loc='best')



plt.show()

# softmax
x = Variable(torch.FloatTensor([[1,4],[-1,4],[2,3]]))
print(x.size())
print(torch.nn.functional.softmax(x,dim =1))
```  

## 加载批次数据集  

因为我们在训练的时候，如果使用了SGD或者你的数据集比较大，我们每次可能只需要喂入网络一个batch，所以我们把我们的数据集进行包装，这样我们每次遍历数据集的时候，就是拿到了一个batch的数据。在torch中我们需要先把数据放到一个DataSet里面，然后用加载器加载一下，以后我们从加载器里面拿到的就是一个batch的数据了。  

```python
import torch
import torch.utils.data as Data
from torch.autograd import Variable
BATCH_SIZE = 3
x = torch.linspace(1,10,10)
y = torch.linspace(10,1,10)
# 转化为DataSet格式
dataset = Data.TensorDataset(x, y)

# 把DataSet放入load进行小批次训练
loader = Data.DataLoader(
    dataset = dataset,
    batch_size=BATCH_SIZE,
    shuffle=True,
    num_workers=2
) # shuffle就是是否打乱，num_workers相当于多线程的意思
for epoch in range(3):
    # 以后从loder里面遍历即可，会反复出来一个个batch，直到遍历一遍数据集
    for step,(batch_x,batch_y) in enumerate(loader):
        print("epoch {}  step {} batch_x:{},batch_y:{}".format(epoch,step,batch_x,batch_y))

```  

## 简单网络搭建以及优化器  

在神经网络中在进行反向传播的时候，有很多种梯度下降的方法，同样的在torch中也提供了出来，这里以一个简单的拟合问题介绍一下网络的搭建以及优化器的使用。   

### 网络搭建

在torch中网络搭建可以通过自己定义一个类继承`torch.nn.Module`，并且重写其中的`__inti__`以及`forward`方法即可，在init我们主要定义一些网络的基本操作，而在forward中主要是把网络串起来。   

```python
class Net(torch.nn.Module):
    def __init__(self,in_n=1,hidden_n=20,out_n=1):
        super().__init__()
        self.hidden = torch.nn.Linear(in_n,hidden_n)
        self.predict = torch.nn.Linear(hidden_n,out_n)
    def forward(self, x):
        x = F.relu(self.hidden(x))
        x = self.predict(x)
        return x
```

上面定义了一个两层小网络，其中在init中一定要先调用父类的init方法，self.hidden定义的是一个线性层，输入是`in_n`个结点，输出是`hidden_n`个结点，self.predict类似。  

下面forward开始进行前向传播，参数x就是输入，先把输入进行线性变换即通过hidden（这里为什么可以把类当做函数用可以参考[另一篇文章][1]，主要用了魔法函数），然后再经过relu激活，下面再进行一次线性变换，最后返回x即可。  

除了上面这种定义一个类的方法，我们还可以利用torch中一种堆叠的方式进行网络结构的定义：  

```python
net = torch.nn.Sequential(
    torch.nn.Linear(1,20),
    torch.nn.ReLU(),
    torch.nn.Linear(20,1)
)
```

上面这个代码就清晰的定义了一个两层结构，先线性，在RELU，最后线性。简单的网路可以通过堆叠构建，复杂的可能就需要自己定义了。  

### 优化器  

在有了网络之后就可以通过优化器以及损失来进行梯度更新了，优化器通过`torch.optim.SGD(net_SGD.parameters(),lr=LR)`来定义，而损失则是先定义一个损失函数，等到使用的时候把数据给损失函数，就可以了。由于是拟合问题，所以用MSE损失`loss_f = torch.nn.MSELoss()`。   

### 完整代码  

``` python 

import torch
import torch.nn.functional as F
import torch.utils.data as Data
from torch.autograd import Variable
import matplotlib.pyplot as plt
LR = 0.01
BATCH_SIZE = 32
EPOCH = 12

x = torch.unsqueeze(torch.linspace(-1,1,1000),dim = 1)
y = x.pow(2) +0.2 * torch.rand(x.size())
# plt.scatter(x.numpy(),y.numpy())
# plt.savefig('./2.jpg')
# x,y = Variable(x),Variable(y)

class Net(torch.nn.Module):
    def __init__(self,in_n=1,hidden_n=20,out_n=1):
        super().__init__()
        self.hidden = torch.nn.Linear(in_n,hidden_n)
        self.predict = torch.nn.Linear(hidden_n,out_n)
    def forward(self, x):
        x = F.relu(self.hidden(x))
        x = self.predict(x)
        return x

net_SGD = Net()
net_Momentum = Net()
net_RMSprop = Net()
net_Adam = Net()

net_SGD = torch.nn.Sequential(
    torch.nn.Linear(1,20),
    torch.nn.ReLU(),
    torch.nn.Linear(20,1)
)
# net_Momentum = torch.nn.Sequential(
#     torch.nn.Linear(1,20),
#     torch.nn.ReLU(),
#     torch.nn.Linear(20,1)
# )
# net_RMSprop  = torch.nn.Sequential(
#     torch.nn.Linear(1,20),
#     torch.nn.ReLU(),
#     torch.nn.Linear(20,1)
# )
# net_Adam  = torch.nn.Sequential(
#     torch.nn.Linear(1,20),
#     torch.nn.ReLU(),
#     torch.nn.Linear(20,1)
# )
nets = [net_SGD,net_Momentum,net_RMSprop,net_Adam]
optimizer_SGD = torch.optim.SGD(net_SGD.parameters(),lr=LR)
optimizer_Momentum = torch.optim.SGD(net_Momentum.parameters(),lr=LR,momentum = 0.8)
optimizer_RMSprop = torch.optim.RMSprop(net_RMSprop.parameters(),lr=LR,alpha=0.9)
optimizer_Adam = torch.optim.Adam(net_Adam.parameters(),lr = LR,betas=(0.9,0.99))
optimizers = [optimizer_SGD,optimizer_Momentum,optimizer_RMSprop,optimizer_Adam]
loss_f = torch.nn.MSELoss()
losses = [[],[],[],[]]

dataset = Data.TensorDataset(x,y)
loader = Data.DataLoader(dataset,batch_size=BATCH_SIZE,shuffle=True,num_workers=2)

for e in range(EPOCH):
    for step,(bx,by) in enumerate(loader):
        # 对多个网络进行分别优化
        for net,optimizer,loss in zip(nets,optimizers,losses):

            out = net(bx)
            loss_v = loss_f(out,by)
            optimizer.zero_grad()
            loss_v.backward()
            optimizer.step()
            loss.append(loss_v.data.numpy())
labels = ["SGD","Momentum","RMSprop","Adam"]
for i,loss in enumerate(losses):
    plt.plot(loss,label=labels[i])
plt.legend(loc='best')
plt.xlabel("steps")
plt.ylabel("loss")
plt.ylim((0,0.2))
plt.savefig("loss1.jpg")

```  

最后会出一张这样的图：  

![image](https://wx4.sinaimg.cn/large/005Dd0fOly1g20qtp7on9j30hs0dctag.jpg)



  [1]: https://blog.csdn.net/qq_28888837/article/details/89193636