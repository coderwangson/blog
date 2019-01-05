# 一步一步使用Tensorflow实现LSTM对mnist分类

标签： `LSTM` `Tensorflow`

---
关于RNN或者LSTM的介绍可以看[这里][1]

## 读入数据集以及定义超参数  

```pyhthon
import tensorflow as tf
from tensorflow.examples.tutorials.mnist import input_data
tf.set_random_seed(1)
# import data
# 读入数据 ./data 代表数据集 位置
mnist = input_data.read_data_sets('./data/',one_hot=True)
# 学习率
lr = 1e-3
# 迭代次数
training_iters = 100000
# 一个批次的大小
batch_size = 128
# 输入的大小 相当于xt的大小
n_inputs = 28
# 对于LSTM要经过多少步，相当于time_steps
n_steps = 28
# 把输入的数据过一下全连接层
n_in_units = 64
#lstmcell的结点个数
n_hidden_units = 128
# 类别个数(0-9)
n_classes = 10
```  
关于n_inputs其实就是输入时候每个$x_t$的大小，而$n\_steps$就相当于有多个$x_t$,对于这个例子刚好所有的$n\_steps$合起来就是一张图片(28*28)。  

## 网络结构  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190105150656372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)  

从图上可以看出来我们除了使用了LSTMcell，还增加了两个全连接层，分别在输入的位置和输出的位置。   

```python   

#define x,y
x = tf.placeholder(tf.float32,[None,n_steps,n_inputs])
y = tf.placeholder(tf.float32,[None,n_classes])
# init eights
weights ={
    # 输入Lstm的最后一维度可以自己定义
    "in":tf.Variable(tf.random_normal([n_inputs,n_in_units])),
    "out":tf.Variable(tf.random_normal([n_hidden_units,n_classes]))
    }
biases ={
    "in":tf.Variable(tf.constant(0.1,shape=[n_in_units])),
    "out":tf.Variable(tf.constant(0.1,shape=[n_classes]))
    }
# define the model
def RNN(X,weights,biases):
    # 3D->2D (batch,n_step,n_inputs)->(batch*n_step,n_inputs)
    X = tf.reshape(X,[-1,n_inputs])
    X_in = tf.matmul(X,weights["in"])+biases["in"]
    # 2D->3D to feed into the RNN cell
    X_in = tf.reshape(X_in,[-1,n_steps,n_in_units])
    # define the RNN cell
    lstm_cell = tf.contrib.rnn.BasicLSTMCell(n_hidden_units)
    init_state = lstm_cell.zero_state(batch_size,dtype = tf.float32)
    # if inputs is (batches, steps, inputs) ==> time_major=False;
    # if inputs is (steps, batches, inputs) ==> time_major=True;
    #  state is (c_state, h_state) and the outputs is equal h_state
    # outputs (batch_size,n_steps,n_hidden_units)
    # final_state is a tuple (c,h) and the c (batch_size,n_hidden_units) ,h(batch_size,n_hidden_units)
    outputs,final_state = tf.nn.dynamic_rnn(lstm_cell,X_in,initial_state=init_state,time_major= False)
    # 从下面代码可以看出 final_state[1] 是h ,相当于最后一个输出
    # ouputs 里面有所有步的输出，相当于所有步的h ,所以 output[-1] 等于 h
#     print(outputs)
#     print(final_state)
#     print(len(tf.unstack(tf.transpose(outputs, [1,0,2]))))
#     print(tf.unstack(tf.transpose(outputs, [1,0,2]))[-1])
#     outputs = tf.unstack(tf.transpose(outputs,[1,0,2]))
#     results = tf.matmul(outputs[-1],weights["out"])+biases["out"]
#  注意与sin那个区分，因为sin那个是每一步其实都代表一个数据，所以要所有步的输出，相当于多对多
#而这里是多步相当于一个图片，所以要最后的状态就行了
    results = tf.matmul(final_state[1],weights["out"])+biases["out"]
    return results

```  

这段代码就定义了前向传播过程，关于这个定义，有几个地方需要指出。  

    lstm_cell = tf.contrib.rnn.BasicLSTMCell(n_hidden_units)
    init_state = lstm_cell.zero_state(batch_size,dtype = tf.float32)  
这两行代码第一行定义了一个`lstm_cell`，有`n_hidden_units`个结点,而第二句定义了初始的输入，一般全零（因为lstm在输入的时候除了x还有一个上层的输出，而对于初始的时候，由于没有上一层的输出，所以用全零）。   

    outputs,final_state = tf.nn.dynamic_rnn(lstm_cell,X_in,initial_state=init_state,time_major= False)  
    
这句代码是代表执行了lstm_cell，并且能连续执行n_steps次，它使用的lstmc_cell就是上面定义的，然后X_in 是一个[batch_size,n_steps,n_in_units]形状的三维数组，而tf.nn.dynamic_rnn之所以知道要执行几次只需要看n_steps为多少就行了，而time_major=False则代表X数据的输入格式是[batch_size,n_steps,n_in_units],如果time_major=True则代表X数据的输入格式是[n_steps,batch_size,n_in_units]。所以这个函数能连续执行n_stpes步，并且把每步执行的输出都存入outputs中，而把最后的状态返回给final_state。  

对于这个例子最终outputs的形状为[bathc_size,n_steps,n_hidden_units]，而final_state是一个tuple里面包含了两个状态一个c一个h，一般我们用h的比较多，其实h就是每步的输出，所以这里final_state中的h的形状为(batch_size,n_hidden_units)，就是相当于最后一个outputs。所以如果我们只需要最后一个输出的时候我们用final_state中的h就行了，如果要全部步的输出的话我们就用outputs,在这个例子里面我们只需要最后一步的输出，因为只有到了最后一步我们才能读完一张图片。   

## 定义损失函数以及开始训练  

```python  
pre  = RNN(x,weights,biases)
# labels=None, logits=None,
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(labels = y,logits = pre))
train_op = tf.train.AdamOptimizer(lr).minimize(cost)
# print(tf.argmax(pre,1)==tf.argmax(y,1))
correct = tf.equal(tf.argmax(pre,1),tf.argmax(y,1))
acc = tf.reduce_mean(tf.cast(correct,tf.float32))
with tf.Session() as sess:
    tf.global_variables_initializer().run()
    step = 0
    while step*batch_size <training_iters:
        batch_xs,batch_ys = mnist.train.next_batch(batch_size)
        batch_xs =batch_xs.reshape([batch_size,n_steps,n_inputs])
        feed_dict={x:batch_xs,y:batch_ys}
        sess.run(train_op,feed_dict=feed_dict)
        if step%20==0:
            print(sess.run([cost,acc],feed_dict=feed_dict))
        step+=1
```  

这里损失就直接使用交叉熵，而优化方法使用AdamOptimizer,后面的都比较简单了，喂入数据就行了，但是在喂入数据前一定要注意修改数据的格式变为[batch_size,n_steps,n_inputs]。

  [1]: https://blog.csdn.net/qq_28888837/article/details/85841924  
  
参考:  

>https://morvanzhou.github.io/tutorials/machine-learning/tensorflow/5-08-RNN2/