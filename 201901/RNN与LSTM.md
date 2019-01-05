# RNN与LSTM

标签： `RNN`

---  

## 什么是RNN   

RNN全称是循环神经网络，其和我们普通的神经网络不同之处在于我们普通的神经网络只有层与层之间有关联，但是RNN每层结点之间也会产生关联，因为这个特性，所以RNN适合用于序列化数据。 
### RNN的表示以及应用  

下面这个图片可能你在很多地方都会见到，这是[国外一个大佬][1]写的。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105104257689.jpg)   

但是这个图片的含义是什么呢，左边一个形态，右边又是一个形态，可能都把我们弄乱了，其实左边和右边是一个东西，只是不同的表达方式，左边那个A上面的箭头是循环的，右边相当于展开了，可以把左边看做是一个递归结构，右边是把递归展开的样子。  

从$x_t到h_t$其实就是我们以前学的神经网络，只不过现在把他竖着画了，以前我们神经网络都是横着的，所以可能刚开始有点迷惑，一旦这个明白了，其实这个结构也有点清晰了，$x_t$ 就是输入，然后$A$就相当于一个全连接层，只是一个特殊的全连接在这里叫做RNNcell，里面进行了特殊的计算，然后$h_t$就是一个输出层，同样的你可以在$x_t$和$A$之间增加一些普通的全连接层，也可以增加几层RNNcell。  

### 前向传播  

定义一个网络，主要在于前向传播以及损失函数的定义，对于RNN的前向传播主要在于RNNcell的计算。下面的图片时一个RNNcell里面的情况：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105105201239.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

可以看出其和我们普通全连接的不同之处在于我们在计算激活tanh的时候运算的除了$x_t$还多了上个结点的的输出，所以这里$h_t = tanh(x_t*w_x + h_{t-1}*w_h+b)$，你可以把$A$看做是一个整体，所以就是我在上一个时段$t-1$计算的值$h_{t-1}$会影响到下一刻$t$的值，所以这就是RNN与传统神经网络的不同之处。   

下面给个例子：  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105110741803.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

这里和上面公式描述的几乎一样，只是是把$h_{t-1}$和$x_t$并起来计算了，并且把$w$也合在一起了，其实如果你使用Tensorflow框架，这个东西时不用你关心的，它会给你一个定义好的Cell，你只要给出关键参数就行了。  

**注意**  

在RNN的那个展开图中我们虽然画了很多个，但是其实中间的RNNcell是共享的，也就是无论你输入多少个序列，我们中间的RNNcell就是那一个。  

### RNN中的参数  

RNN的输入是一个序列，所以在输入的时候是一个三维个格式[batch_size,time_steps,input_size],其中batch_size的含义就是批处理把数据分为多少批次，time_steps就是时间序列的长度，也就相当于把RNN展开多少个，input_size相当于x_t的大小，我们在定义RNNcell的时候需要给出一个cell_size这个代表经过RNNcell后输出几个结点，也就相当于我们神经网络中隐含层的单层有几个结点，除此之外我们还需要一个初始的$h_0$，因为在第一个时刻前面没有输出，一般我们用全0代表$h_0$。  

## LSTM  

LSTM全称长短记忆网络(long short-term memory)，这个网络可以解决RNN在时间序列过长的时候初始的数据，因为如果时间序列过长，在最后预测的时候由于距离初始的输入过长，所以导致有些数据忘记，而LSTM可以避免这个情况。  

LSTM的原理和RNN是类似的，只是在RNNcell基础上进行了改造，把他改成LSTMcell，主要是增加了一些控制门，这样的话能保证不忘记前面的数据。  

### LSTM结构  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105112127220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

可以看出LSTM和RNN的区别之处主要在于中间那个CELL的不同，但是如果你使用的是Tensorflow你几乎不用关心这个东西，因为Tensorflow中也给出了这个CEll的定义，你可以直接进使用。  

LSTM的cell的主要结构是三个门进行控制的。  

#### 遗忘门  

LSTM的第一步是决定要从上一个时刻的状态中丢弃什么信息，其是由一个sigmoid全连接的前馈神经网络的输出管理，将这种操作称为遗忘门（forget get layer）。这个全连接的前馈神经网络的输入是$h_{t-1}$和$X_t$组成的向量，输出是$f_t$向量。$f_t$向量是由1和0组成，1表示能够通过，0表示不能通过。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105112542972.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)   

#### 输入门  

第二步决定哪些输入信息要保存到神经元的状态中。这由两个前馈神经网络。首先是一个`sigmoid`层的全连接前馈神经网络，称为输入门（input gate layer），其决定了哪些值将被更新；然后是一个`tanh`层的全连接前馈神经网络，其输出是一个向量$C_t$，$C_t$向量可以被添加到当前时刻的神经元状态中；最后根据两个神经网络的结果创建一个新的神经元状态。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105112755398.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

#### 状态控制  

第三步就可以更新上一时刻的状态$C_{t-1}$为当前时刻的状态$C_t$了。上述的第一步的遗忘门计算了一个控制向量，此时可通过这个向量过滤了一部分$C_{t-1}$状态，如下图 所示的乘法操作；上述第二步的输入门根据输入向量计算了新状态，此时可以通过这个新状态和$C_{t-1}$状态根据一个新的状态$C_t$，如下图所示的加法操作。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105113013453.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

#### 输出门  

最后一步就是决定神经元的输出向量$h_t$是什么，此时的输出是根据上述第三步的$C_t$状态进行计算的，即根据一个sigmoid层的全连接前馈神经网络过滤到一部分$C_t$状态作为当前时刻神经元的输出，如下图所示。这个计算过程是：首先通过sigmoid层生成一个过滤向量；然后通过一个tanh函数计算当前时刻的$C_t$状态向量（即将向量每个值的范围变换到[-1,1]之间）；接着通过sigmoid层的输出向量过滤tanh函数结果，即为当前时刻神经元的输出。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105113227252.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

## 总结  

其实RNN理解起来并不是太难，但是在应用的时候会把自己弄混乱，因为RNN的输入是一个序列，关于这个序列可能是一个在时间流上的内容，所以和之前用的CNN是在空间上的内容有点差别，所以在搭建RNN的时候会发现在输入的维度，输出的维度以及隐含层的维度这些东西的定义有时候会弄混乱，而至于RNNcell里面的具体计算，在使用Tensorflow的时候不需要关心，因为Tensorflow已经帮你实现了。







  [1]: http://colah.github.io/posts/2015-08-Understanding-LSTMs/  
  
  参考：  
  
\[1]: http://colah.github.io/posts/2015-08-Understanding-LSTMs/  
\[2]: https://www.cnblogs.com/huliangwen/p/7464813.html
\[3]: https://morvanzhou.github.io/tutorials/machine-learning/tensorflow/5-08-RNN2/ 
\[4]: https://mooc.study.163.com/learn/2001280005?tid=2001391038#/learn/content?type=detail&id=2001771053&cid=2001777006
  
  