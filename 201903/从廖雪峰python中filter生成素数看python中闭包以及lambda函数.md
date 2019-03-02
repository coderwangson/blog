# 从廖雪峰python中filter生成素数看python中闭包以及lambda函数

标签： `python`

---

廖雪峰使用filter生成素数的例子在https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431821084171d2e0f22e7cc24305ae03aa0214d0ef29000 。  

代码为：  

```python
def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n
def _not_divisible(n):
    return lambda x: x % n > 0
def primes():
    yield 2
    it = _odd_iter() # 初始序列
    while True:
        n = next(it) # 返回序列的第一个数
        yield n
        it = filter(_not_divisible(n), it) # 构造新序列
# 打印1000以内的素数:
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```  

对于第一个函数来说，就是做了一个生成器，能不断生成奇数（从3开始），然后primes也是一个生成器，是用来生成素数的。利用的是埃氏筛法。首先生成第一个素数2，然后使用奇数生成器`it`，然后产生第一个素数`n=3`，然后使用了一个过滤器，这个过滤器作用就是判断it里面的数字是否是`n`的倍数`(x % n > 0)`,这个地方令人不解的是，我把过滤器里面的`_not_divisible(n)`换成`lambda x: x % n > 0`就不行了，生成的序列就不正确，后来查看了网上发现一个说的是
>lambda中n是引用（所以会变，都是同一个）而经过函数的 n 会被拷贝一份放闭包里。闭包是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。  

这段话当时看的不是很明白，不过后来自己想了想，也有点想清楚了，我下面表达的不一定对，但是能解释为什么会出现这样的情况。    

对于这个例子，首先有一个奇数生成器如下图：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190302140253645.png)   

一定要注意这是一个**生成器**而不是一个列表，对于这个生成器来说，能产生无数个数字。  

然后再下面的`primes()`函数里面我们使用了这个生成器，先生成了一个3，后面我们使用过滤器，对于这个过滤器而言，第二个参数也是一个生成器，所以过滤器不可能一下子把所有数据都作用上过滤器中的函数`_not_divisible(n)`,所以在这里只是记录了一个规则。   

此时我们生成器的next来到了5，然后我们取数据的时候，先对这个5作用一下上面的规则，如果满足这个过滤器的这个规则我们就把5给生成出来，否则继续直到满足规则。  

之所以直接写lambda不行是因为在lambda中n是一个引用，n一直跟着外面的n变化，所以我们每次作用规则的时候n是一直在变化的。  

而在函数中，是一个闭包，你也可以看做是单独的一块空间，每次作用规则时候n是不同的，所以就能产生素数。  

下面再给个例子:  

```python
def gen():
    n = 1
    yield n
    while True:
        n +=1
        yield n
def f(n):
    return lambda x:x==n+1
def p():
    g = gen()
    while True:
        n = next(g)
        yield n
        g = filter(f(n),g)
h = p()
print(next(h))
print(next(h))
print(next(h))
print(next(h))
print(next(h))
print(next(h))
```  

1、分析如果过滤器传入的是函数f(n)的情况     

首先有个生成器，当`n = next(g)`的时候，产生了一个1，即n=1。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190302141129792.png)  

然后定义了过滤器的规则，当x ==2的时候保留，所以等到下次我们在n = next(g)的时候，刚好n为2，所以满足，所以输出，然后又定义规则当x==3,注意这个x==3的规则是在一个函数fn中，而x==2也是在一个函数fn中，这两个函数在内存中互相不干扰，所以在执行到n = next(g)，n是3，其不满足==2 and ==3，所以不输出，一直向后找，发现找不到，所以程序没有输出。

2、分析直接传入 lambda x:x==n+1  

而传入lambda x:x==n+1 ，则同样先产生一个1，然后规则是x==2，而下次再生成的时候n刚好为2，所以输出，然后定义规则x==3，但是之前的规则没有了，因为n都是同一个n，现在n变为了3而已，注意和上面的去别，上面的是一个新的函数，所以n单独开辟了，而这里的n则不同。所以当下次执行的时候刚好是3，所以输出。以后则同。


参考：
https://blog.csdn.net/cjsyr6wt/article/details/78123264




