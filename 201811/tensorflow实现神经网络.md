## tensorflow实现神经网络   

#### 1、全部步骤   

1. 实现前向传播
2.  声明学习率 
3. 参数进行正则化计算
4. 计算损失函数
5. 反向传播
6. 参数进行滑动平均    

#### 2、各个步骤解释  

前向传播：主要是定义这个网络的结构，网络是几层的，以及每层使用的激活函数是什么，一般前向传播完成后，这个网络结构就已经完成。  

声明学习率 ：学习率是定义了进行梯度下降时下降的速率  

参数进行正则化计算、 计算损失函数：这两个可以放在一起说，因为这两个合在一起就是最终要优化的函数，，正则化和损失函数合在一起是最终的优化函数，其中正则化（防止过拟合）可以省略，但是如果使用的话可以采用L1或者L2正则化，其中L1、L2正则化只是一种计算的方式，你可以把其看做两个函数，对于损失函数来说，可以使用均方误差，也可以使用交叉熵进行计算。   

反向传播：主要是更新参数，因为要寻找能使损失函数最小的参数值，所以采用一种方式进行不断的调整参数（一般使用梯度下降等方式）     

参数进行滑动平均：这也是用在参数上面的，其主要目的是是的模型更加健壮。  注意的是：进行滑动平均的参数用来计算准确率，没有滑动平均的参数进行计算损失函数。

#### 3、各个步骤在Tensorflow中涉及到的API   

前向传播：主要是一个矩阵乘还有就是激活函数。tf.matmul()、tf.nn.relu()    

学习率：学习率可以只是一个常数，那么就直接learning_rate = 0.03就行了，还有一种是指数衰减学习率，这个时候就需要借助一个tf中的函数了tf.train.exponential_decay()，其具体使用可以自己参考tf官方api也可以自己网上搜索一下。   

正则化：正则化主要是对参数进行正则化，使用regularizer = tf.contrib.layers.l2_regularizer(REGULARIZATION_RATE)可以产生一个函数regularizer ，这个函数可以对别的参数进行正则化 ：regularizer(weights1)    

损失函数：可以使用均方误差，使用交叉熵，也可以自定义，这里给出交叉熵的tf.nn.softmax_cross_entropy_with_logits_v2，还要注意最后要进行求均值，tf.reduce_mean，还要注意tf.nn.sparse_softmax_cross_entropy_with_logits和tf.nn.softmax_cross_entropy_with_logits_v2的区别。  

>https://blog.csdn.net/u013084616/article/details/79138380   

反向传播：反向传播其实就是更新参数的过程，一般使用梯度下降tf.train.GradientDescentOptimizer(learning_rate).minimize(loss,global_step = global_step)，从这个api中也可以看出里面有学习率，还有损失函数（loss就是损失函数的定义）。所以这个api的作用就是以learning_rate为学习率，对loss函数进行最优化。   

参数进行滑动平均：主要也是对参数的，  
	
		#定义一个滑动平均类		
		average_class=tf.train.ExponentialMovingAverage(MOVING_AVERAGE_DECAY,global_step)
		#定义一个滑动平均操作
	    average_class_op = average_class.apply(tf.trainable_variables())   

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105194354267.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)


#### 4、代码实现上面过程-参考Tensorflow实战Google深度学习框架  
	
	'''
	Created on 2018年11月5日
	
	@author: coderwangson
	'''
	"#codeing=utf-8"
	
	from tensorflow.examples.tutorials.mnist import input_data
	import tensorflow as tf
	
	
	INPUT_NODE = 784
	OUTPUT_NODE = 10
	
	LAYER1_NODE = 10
	
	BATCH_SIZE = 100
	
	LEARNING_RATE_BASE = 0.8
	LEARNING_RATE_DECAY = 0.99
	
	REGULARIZATION_RATE = 1e-4
	TRAING_STEP = 30000
	MOVING_AVERAGE_DECAY = 0.99
	
	# 进行前向传播计算
	
	def inference(input_tensor,avg_class,weights1,bias1,weights2,bias2):
	    if avg_class ==None:
	        layer1 = tf.nn.relu(tf.matmul(input_tensor,weights1)+bias1)
	        return tf.matmul(layer1,weights2)+bias2
	    else:
	        layer1 = tf.nn.relu(tf.matmul(input_tensor,avg_class.average(weights1))+avg_class.average(bias1))
	        return tf.matmul(layer1,avg_class.average(weights2))+avg_class.average(bias2)
	
	
	def train(mnist):
	    x = tf.placeholder(tf.float32, [None,INPUT_NODE], "x_input")
	    y_ = tf.placeholder(tf.float32,[None,OUTPUT_NODE],"y_input")
	    
	    weights1 = tf.Variable(tf.truncated_normal([INPUT_NODE,LAYER1_NODE],stddev=0.1))
	    bias1 = tf.Variable(tf.constant(0.1,shape = [LAYER1_NODE]))
	    weights2 = tf.Variable(tf.truncated_normal([LAYER1_NODE,OUTPUT_NODE],stddev=0.1))
	    bias2 = tf.Variable(tf.constant(0.1,shape = [OUTPUT_NODE]))
	    
	    # 前向传播	   
	    # 没有使用滑动平均
  		y = inference(x,None,weights1,bias1,weights2,bias2) 
	    global_step = tf.Variable(0,trainable=False)
	    average_class = tf.train.ExponentialMovingAverage(MOVING_AVERAGE_DECAY,global_step)
	    average_class_op = average_class.apply(tf.trainable_variables())
	#     使用滑动平均
	    average_y = inference(x,average_class,weights1,bias1,weights2,bias2)
	    
	    #损失函数  sparse_softmax_cross_entropy_with_logits适用于稀疏矩阵的时候
	    """
	        两个函数虽然功能类似，但是其参数labels有明显区别。
	   tf.nn.softmax_cross_entropy_with_logits（）
	        中的logits和labels的shape都是[batch_size, num_classes]，
	        而tf.nn.sparse_softmax_cross_entropy_with_logits（）中的labels是稀疏表示的，
	        是 [0，num_classes）中的一个数值，代表正确分类结果。
	        即sparse_softmax_cross_entropy_with_logits 直接用标签计算交叉熵，
	        而 softmax_cross_entropy_with_logits 是标签的onehot向量参与计算。
	   softmax_cross_entropy_with_logits 的 labels 是 sparse_softmax_cross_entropy_with_logits 的 labels 的一个one hot version。
	"""
	    
		#损失函数
	    cross_entropy = tf.reduce_mean(tf.nn.sparse_softmax_cross_entropy_with_logits(labels=tf.argmax(y_, 1), logits=y))
	    
	    #正则化
	    regularizer = tf.contrib.layers.l2_regularizer(REGULARIZATION_RATE)
	    regularization = regularizer(weights1) + regularizer(weights2)
	    
	    loss = cross_entropy+regularization
	    
	    #学习率
	    learning_rate = tf.train.exponential_decay(LEARNING_RATE_BASE,global_step ,mnist.train.num_examples/BATCH_SIZE,LEARNING_RATE_DECAY)
	    
	    # 反向传播
	    train_step = tf.train.GradientDescentOptimizer(learning_rate).minimize(loss,global_step = global_step)
	    
		#因为两个都要更新参数，并且二者是独立的（参考上面那个图），所以把二者合成一个操作，方便sess.run()
	    with tf.control_dependencies([train_step,average_class_op]):
	        train_op = tf.no_op(name = 'train')
	        
	    correct_prediction = tf.equal(tf.arg_max(average_y,1),tf.argmax(y_,1))
	    
	    accuracy  = tf.reduce_mean(tf.cast(correct_prediction,tf.float32))
	    
	    with tf.Session() as sess:
	        sess.run(tf.global_variables_initializer())
	        
	        validate_feed = {x:mnist.validation.images,y_:mnist.validation.labels}
	        test_feed = {x:mnist.test.images,y_:mnist.test.labels}
	        
	        for i in range(TRAING_STEP):
	            if i%1000 ==0:
	                validate_acc = sess.run(accuracy,feed_dict = validate_feed)
	                print("this is %dth acc is %g"%(i,validate_acc))
	        
	            xs,ys = mnist.train.next_batch(BATCH_SIZE)
	            sess.run(train_op,feed_dict = {x:xs,y_:ys})
	  
	            
	        test_acc = sess.run(accuracy,feed_dict = test_feed)
	        print("the test acc is %g"%test_acc)
	
	
	    
	    
	mnist = input_data.read_data_sets("./data",one_hot=True)
	# print(mnist.test.num_examples)
	train(mnist)    
    









 
