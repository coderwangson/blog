# pytorch实现Resnet

标签： `pytorch` `resnet`

---

## 网络结果及其基本单元  

对于Resnet来说基本，只要把其基本结构抽离出来即可，其他的其实和以前我们的普通卷积神经网络很像。而Resnet中最基本的结构就是一个残差块如下：    

![image](https://ws1.sinaimg.cn/large/005Dd0fOly1g2qj3s0dtvj30bi0673zn.jpg)  

可以看出一个残差块分为左右两部分，左边其实就是普通卷积操作，而右边什么都没有（不过在实际中会有一个卷积），然后输出就是两个的和。  
所以一个对于一个输入x [batch,c,h,w] 来说经过左边可能变为了[batch,c1,h1,w1],同样右边也使用其他的卷积变为[batch,c1,h1,w1]，然后加起来即可。  

之所以要做一个短连接的作用是避免网络层数过深导致的梯度爆炸或者消失，对于一个残差块，我们能保证最坏的结果就是左边的卷积不起作用了，这样无论网络再深，其实只相当于还是x，所以就能保证不造成梯度问题。具体细节可以参考论文。  

## pytorch实现  

### 定义残差块  

```python
    class ResidualBlock(nn.Module):
        """实现一个残差块"""
        def __init__(self,inchannel,outchannel,stride = 1,shortcut = None):

            super().__init__()
            self.left = nn.Sequential(
                nn.Conv2d(inchannel,outchannel,3,stride,1,bias=False),
                nn.BatchNorm2d(outchannel),
                nn.ReLU(),
                nn.Conv2d(outchannel,outchannel,3,1,1,bias=False), # 这个卷积操作是不会改变w h的
                nn.BatchNorm2d(outchannel)
            )
            self.right = shortcut

        def forward(self, input):
            out = self.left(input)
            residual = input if self.right is None else self.right(input)
            out+=residual
            return F.relu(out)
```  

可以看出就是恩威左右两个部分，左边就是普通的卷积操作，而右边当前没有定义，可能是None，如果是None则相当于直接是原来的x不经过任何操作，也或者是经过一些操作。  

在左边的定义中第二个卷积层的参数是`3 1 1`这个卷积操作是不会改变w h的。 

### Resnet  

有了残差块，就可以直接定义Resnet了。  


```python
    class ResNet(nn.Module):
        """实现主reset"""

        def __init__(self,num_class=1000):
            super().__init__()
            # 前面几层普通卷积
            self.pre = nn.Sequential(
                nn.Conv2d(3,64,7,2,3,bias=False),
                nn.BatchNorm2d(64),
                nn.ReLU(),
                nn.MaxPool2d(3,2,1)
            )

            # 重复layer，每个layer都包含多个残差块 其中第一个残差会修改w和c，其他的残差块等量变换
            # 经过第一个残差块后大小为 w-1/s +1 （每个残差块包括left和right，而left的k = 3 p = 1，right的shortcut k=1，p=0）
            self.layer1 = self._make_layer(64,128,3) # s默认是1 ,所以经过layer1后只有channle变了
            self.layer2 = self._make_layer(128,256,4,stride=2) # w-1/s +1
            self.layer3 = self._make_layer(256,512,6,stride=2)
            self.layer4 = self._make_layer(512,512,3,stride=2)
            self.fc = nn.Linear(512,num_class)

        def _make_layer(self,inchannel,outchannel,block_num,stride = 1):

            # 刚开始两个cahnnle可能不同，所以right通过shortcut把通道也变为outchannel
            shortcut = nn.Sequential(
                # 之所以这里的k = 1是因为，我们在ResidualBlock中的k =3,p=1所以最后得到的大小为(w+2-3/s +1)
                # 即(w-1 /s +1)，而这里的w = (w +2p-f)/s +1 所以2p -f = -1 如果p = 0则f = 1
                nn.Conv2d(inchannel,outchannel,1,stride,bias=False),
                nn.BatchNorm2d(outchannel)
            )

            layers = []
            layers.append(ResidualBlock(inchannel,outchannel,stride,shortcut))

            # 之后的cahnnle同并且 w h也同，而经过ResidualBloc其w h不变，
            for i in range(1,block_num):
                layers.append(ResidualBlock(outchannel,outchannel))

            return nn.Sequential(*layers)


        def forward(self, input):
            x = self.pre(input)

            x = self.layer1(x)
            x = self.layer2(x)
            x = self.layer3(x)
            x = self.layer4(x)

            x = F.avg_pool2d(x,7) # 如果图片大小为224 ，经过多个ResidualBlock到这里刚好为7，所以做一个池化，为1，
                                # 所以如果图片大小小于224，都可以传入的，因为经过7的池化，肯定为1，但是大于224则不一定
            print(x.shape)
            x = x.view(x.size(0),-1)
            return self.fc(x)
````  

这段代码中关键在于`_make_layer`，其实就是多个残差块构成了一个大层，所以叫做`_make_layer`。  

这个网络刚开始是一些普通的卷积操作，可以看做一些预处理，然后开始做几个残差层。在做残差层的时候，关键在于如果大小不匹配怎么办，所以在我们的残差层里面，如果有多个残差块的时候，我们第一个残差块是带shorcut的，因为第一个ResidualBlock是传入了stride的，所以可能造成w和h的变化，而右边经过shortcut则通过计算是刚好能和左边保持一致的，所以能直接加，而后面的几个残差块是没有传入stride的，经过计算发现left的结果会保持输入的w h 都不变，所以shortcut可以为None。  

其实对于这个代码主要是计算的问题，要学会如何计算卷积的w和h，其他的就没什么了，对于一个残差层，包括多个残差块，第一个残差块可以改变w h，而后面的残差块则是不变的。  









