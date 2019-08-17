# pytorch第二弹

标签（空格分隔）：  `pytorch` `深度学习`

---

## pytorch中的numpy  

深度学习是以矩阵为基础的，里面的前向传播都是用矩阵进行计算的，所以在深度学习框架里面需要大量的矩阵操作以及定义。学过python的都知道，在python里面有一个科学工具包numpy是专门来操作矩阵的，在pytroch里面也有类似的操作，pytorch里面使用Tensor代替numpy。numpy能做的事情，Tensor都能完成，并且二者可以互相转换，非常方便。  

## Tensor定义  

```python
x = torch.Tensor(5,3)
print(x.size())
```

Tensor的定义非常简单，并且和numpy很多地方类似，比如上面的操作语句就定义了一个5行3列的矩阵。你可以通过`.size()`来看矩阵的形状。  

### Tensor四则运算  

```python
x = torch.Tensor(5,3)
y = torch.rand(5,3)
print(x+y)
print(torch.add(x,y))
result = torch.Tensor(5,3)
print(torch.add(x,y,out = result)) # 结果在result里面
print(result)
```  

注意四则运算时矩阵的形状非常重要，比如进行加法操作的时候，两个矩阵要大小一致，进行乘法要内标相同。  

四则运算可以直接使用操作符`+ -`,也可以使用一些函数`add mm`等。 在用函数的时候加了out参数，则运算结果会存储到out里面。  

除了`add mm`这种类型的函数之外，还有`add_`下面带下划线的，他们功能时一样的，只是add_会把结果覆盖原来的：

    y.add_(x)  
    
则y的值会被修改。  

### 与numpy转换  

其实Tensor里面的数据本质还是一个numpy，只是在上面进行了一些封装，为了方便torch框架识别。  

把一个Tensor转为numpy通过下面的方法：  

    y = x.numpy()
    
把一个numpy转为Tensor可以通过下面的方法：  

    x = torch.from_numpy(y)  
    
## Variable  

这里只会简单介绍一下Variable，因为Variable是和自动微分结合在一起的，自动微分也就是自动求导，在之前讲过，这些深度学习框架的主要厉害之处就在于能过自动帮你进行反向传播，而反向传播的基础就是自动微分。   

Variable本质是把一个Tensor进行了封装，只是具备了能够微分的属性，所以可能在数值上看不出来差异。  

```python
from torch.autograd import Variable
x = Variable(torch.ones(2,2),requires_grad = True)
print(x)
y = 4* x.sum()
print(y.grad_fn)
r = x.sum()
print(r.grad_fn)
```  

删改你定义了一个x是一个Variable，里面的数值就是2行2列的1，然后用`y = 4 * （sum(x)）`，也就是把x相加然后乘以4，这里y也是一个Variable，下面的`y.grad_fn`体现了和Tensor的不同，在Variable可以看到他的导函数，这个grad_fn就可以用来看  

![image](http://ws2.sinaimg.cn/large/005Dd0fOly1g5s3tnqrb6j30as02ymx1.jpg)  

可以看出输出了y的导函数是一个乘法操作，而r的导函数是一个加法，通过这个torch就能一步步的进行求导了。   


总结：  这一次学习了基本的Tensor的操作，可以尝试一些别的矩阵操作，比如矩阵转置，求奇异矩阵等，使用torch进行实现。如果有些操作在torch里面没有，可以尝试把Tensor先转为numpy，通过numpy计算完成后，再转为Tensor，可以说torch是非常的灵活。


