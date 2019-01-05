# Tensorflow入门

标签： `tensorflow`

---
安装Tensorflow参考[这篇文章][1]
## 关于Tensorflow  

Tensorflow是Google的一款产品，主要用于神经网络搭建。在使用Tensorflow的时候，要注意其和以前你写的代码的区别，Tensorflow是先定义了一波计算方式***（也就是把许多变量以及变量间运算存储成计算图）**，然后开启一个会话(Session)，我们在会话里面计算这个图。所以当初我在学习的时候很不习惯，我总是定义一个变量后就直接进行print，会发现输出的就是一串字符，也不知道是干什么的，其实这个就是Tensorflow的特别的地方，它是想定义，最后使用的时候是在Session中使用，所以一定要接受这个观念。   

## 张量 

在Tensorflow中它自己规定了定义变量的方法，就是如果你想使用Tensorflow进行计算，那么你定义变量的时候必须使用Tensorflow的类型定义。看下面的代码：   

```python  
a = tf.constant(10,tf.float32)
b = tf.constant(11,tf.float32)
c = a+b
print(a)
print(c)

```  

这段代码的意思就是定义了两个张量，`constant`的意思是常数（后面还会学到变量的），然后c是a和b相加，如果你按照正常的思维，你会觉得第一个`print`应该输出一个`10`一个`21`，但是结果却是两串字符串：  

    Tensor("Const:0", shape=(), dtype=float32)
    Tensor("add:0", shape=(), dtype=float32)  
    
这也证实了我上面说的，Tensorflow前面是定义一些东西，也就是定义了一个计算图（计算的方式），然后在会话里面才真正的执行。上面两个字符串的意义是，a是一个张亮，名字是`Const:0`，因为是常量，所以形状是`()`，类型是`float32`，所以上面只给出了关于a的定义，就相当于搭了一个架子，上面就相当于搭了如下的架子：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105093633328.png)  

## 会话  

当我们把架子搭完了（比如你神经网络前向传播定义好了），你就开始拿到会话中计算，所以我们要开启一个会话，然后计算我们的计算图，代码如下：  

```python
a = tf.constant(10,tf.float32)
b = tf.constant(11,tf.float32)
c = a+b
with tf.Session() as sess:
    print(sess.run(a))
    print(sess.run(c))
```  

上面定义`a,b,c`还是依旧，只是下面用了一句`with tf.Session() as sess:` 这句话就相当于开启了一个会话，以后你就可以使用`sess`
进行计算图了，然后下面`sess.run(a)`就相当于计算a看一下a的值，最后输出的是10.0（因为是float32类型）。  

## 扩展-其他的数据类型  

其实有了上面的基础就算是对Tensorflow有了直观的感受，后续就可以直接看一下Tensorflow有哪些函数，然后去使用就行了。  

下面讲的是对Tensorflow其他数据类型的讲解，因为只有一个`tf.constant`是不够的，所以Tensorflow引入了`tf.Variable()`，这个定义的是一个变量，里面有参数，不同的参数代表生成不同形态的变量。  

```python
a = tf.Variable(tf.random_normal([3,2]))
b = tf.Variable(tf.zeros([3,2]))
```  

a代表是一个符合正态分布，形状为3行2列的变量，b是生成了全部为0，形状为3行2列的变量，其中`tf.random_normal`和`tf.zeros`是Tensorflow中的生成函数，有如下这些常用类型：   

TensorFlow 随机数生成函数  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105095026612.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

TensorFlow 常数生成函数  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105095101298.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

**注意**  

因为是变量，尤其是像随机数生成的，由于他们在初始的时候并没有值，只是定义了结构，所以你在会话中使用这些变量的时候，一定一定要注意进行初始化。如果你报如下异常，就说明你忘记初始化了：  

    tensorflow.python.framework.errors_impl.FailedPreconditionError: Attempting to use uninitialized val
    ue Variable  
    
你可以使用如下方法初始化：  

1. 初始化单个变量 

    sess.run(a.initializer)
2. 初始化所有变量  

    sess.run(tf.global_variables_initializer())



  [1]: https://blog.csdn.net/qq_28888837/article/details/80757731