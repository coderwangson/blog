#  torch.nn.Linear 与魔法属性

标签： `pytorch`

---

这篇文章不是说torch.nn.Linear这个类怎么用，因为网上有大把的东西，而是想通过这个引申出一个关于`python`中的魔法函数。  

如果你学过torch，你肯定知道我们建立层可以通过`hidden = torch.nn.Linear(n_feature,n_hidden)`建立一个单层的网络，但是下面我们使用的时候可能你就看不太懂了，我们调用`hidden(x)`，这个地方我开始迷惑很久，因为`hidden`其实是一个类对象，但是我下面使用`hidden(x)`好像把hidden当做了一个函数，其实这里是使用了`python`中的一个魔法函数`__call__`。看下面一段代码：  

```python
class A(object):
    def __call__(self, *args, **kwargs):
        print("call me")
a = A()
a()
``` 

最终会输出`call me`，这个和上面那个效果一样，并且在pytorch里面还有很多地方都这样用的，比如我们重写了前向传播，但是使用的时候确直接使用网络的`类对象()`这样就可以了，这里也是`__call__`函数帮你做的，所以要熟悉一下python中的这些魔法属性，要不然到时候为什么都不知道，怎么读源代码呢。  

参考:  

https://stackoverflow.com/questions/54518808/pytorch-weak-script-method-decorator




