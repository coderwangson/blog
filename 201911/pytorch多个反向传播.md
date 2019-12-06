# pytorch多个反向传播

标签： `pytorch`

---

之前我的一篇文章`pytorch 计算图以及backward`，讲了一些pytorch中基本的反向传播，理清了梯度是如何计算以及下降的，建议先看懂那个，然后再看这个。   

从一个错误说起：`RuntimeError: Trying to backward through the graph a second time, but the buffers have already been freed`  

在深度学习中，有些场景需要进行两次反向，比如Gan网络，需要对D进行一次，还要对G进行一次，很多人都会遇到上面这个错误，这个错误的意思就是尝试对一个计算图进行第二次反向，但是计算图已经释放了。其实看简单点和我们之前的backward一样，当图进行了一次梯度更新，就会把一些梯度的缓存给清空，为了避免下次叠加，但在Gan这种情形下，我们必须要二次更新，那怎么办呢。有两种方案：  

方案一：  

这是网上大多数给出的解决方案，在第一次反向时候加入一个`l2.backward()`,这样就能避免释放掉了。  

方案二：  

上面的方案虽然解决了问题，但是并不优美，因为我们用Gan的时候，D和G两者的更新并无联系，二者的联系仅仅是D里面用到了G的输出，而这个输出一般我们都是直接拿来用的，而问题就出现在这里。下面给一个模拟：  

```
data = torch.randn(4,10)

model1 = torch.nn.Linear(10,2)
model2 = torch.nn.Linear(2,2)

optimizer1 = torch.optim.Adam(model1.parameters(), lr=0.001,betas=(0.5, 0.999))
optimizer2 = torch.optim.Adam(model2.parameters(), lr=0.001,betas=(0.5, 0.999))

loss = torch.nn.CrossEntropyLoss()
data = torch.randn(4,10)
label = torch.Tensor([0,1,1,0]).long()
for i in range(20):
    a = model1(data)
    b = model2(a)
    l1 = loss(a,label)
    l2 = loss(b,label)
    optimizer2.zero_grad()
    l2.backward()
    optimizer2.step()

    optimizer1.zero_grad()
    l1.backward()
    optimizer1.step()
```  

上面定义了两个模型，而model2的输入是model1的输出，而更新的时候，二者都是各自更新自己的参数，并无联系，但是上面的代码会报一个`RuntimeError: Trying to backward through the graph a second time, but the buffers have already been freed` 这样的错，解决方案可以是`l2.backward(retain_graph=True)`。除此之外我们还可以是`b = model2(a.detach())`,这个就优美一点，`a.detach()`和`a`的区别你可以打印出来看一下，其实`a.detach()`是没有梯度的，所以相当于一个单纯的数字，和model1就脱离了联系，这样model2和model1就是完全分离开来的两个图，但是如果用的是`a`则model2和model1则仍然公用一个图，所以导致了错误。可以看下面示意图（这个是我猜测，帮助理解）：  

![2019-11-26_101938.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1g9b86upccrj30dx0800t0.jpg)  

左边相当于直接用`a`而右边则用`a.detach()`，类似的在Gan网络里面D的输入可以改为G的输出`y_fake.detach()`。  

但有一点需要注意的是，两个网络一定没有需要共同更新的 ，假如上面的`optimizer2 = torch.optim.Adam(itertools.chain(model1.parameters(),model2.parameters()), lr=0.001,betas=(0.5, 0.999))`,则还是用`retain_graph=True`保险，因为.detach则model2反向不会传播到model1，导致不对model1里面参数更新。



