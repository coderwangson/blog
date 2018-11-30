##  使用tensorflow模拟神经网络  

### 一、安装tensor并且导入   

如何安装tensor前面介绍过了，导入直接使用
	
	import tensorflow as tf  

### 二、准备数据集  

这里就自己生成一批数据，并且把数据用一个圆分开，圆内代表一类，圆外代表一类。  
	
	D = 2 # 输入X的维度，也就是x有几类特征
	K = 300 # 生成点的个数
	C = 2 # 输出Y有几类
	# 准备数据集
	X  = (np.random.random((K,D))*10) -5
	# Y_c代表类别，将来画图可以用
	Y_c = [int(x0*x0+x1*x1<9) for (x0,x1) in X]
	# Y是实际的输出，如果圆内和圆外可以用[1,0]与[0,1]表示
	# 这里做了分类，因为Y要么圆内，要么圆外面，所以做了分类得到one-hot数据格式
	Y = pd.get_dummies(Y_c).values   

### 三、 网络模型构建  

在tensor中是先把模型构建出来（输入、中间神经层、输出等），构建完模型最后在进行统一的运算。  

这里我们构建一个中间有一个为100的隐含层，在模型构建的过程中主要就是确定维度，维度的确定主要就是要了解矩阵乘法规则，然后根据输入X和输出Y的维度，来推算中间层可以的维度。  

	def get_weight(shape,reg):
	    #创建一个厨师的权重值，可以随机
	    w = tf.Variable(tf.random_normal(shape),dtype = tf.float32)
	    # 如果进行正则化，则需要把损失值加到损失值里面
	    tf.add_to_collection('losses',tf.contrib.layers.l2_regularizer(reg)(w))
	    return w
	def get_bais(shape):    
	    # b一般初始化为0
	    return tf.Variable(tf.constant(0.0,shape = shape,dtype = tf.float32))
	# 开始构建神经图模型
	# 输入输出占位
	x = tf.placeholder(dtype=tf.float32, shape=(None,D))
	y = tf.placeholder(dtype=tf.float32, shape=(None,C))
	h = 100
	# 构建网络 
	w1 = get_weight([D,h],1e-3)
	b1 = get_bias([1,h])
	# 隐含层要使用激活函数relu
	y1 = tf.nn.relu(tf.matmul(x,w1)+b1)
	
	w2= get_weight([h,C], 1e-3)
	b2 = get_bias([1,C])
	y_ = tf.matmul(y1,w2)+b2  

两个函数get_weight和get_bias主要是方便生成w和b所以封装成函数格式，参数shape就是生成w或者b的矩阵的形状，是几行几列的，而reg代表正则化，就像之前讲的神经网络中的正则化是一样的，都是为了对w进行惩罚，避免过拟合。  

然后 x = tf.placeholder(dtype=tf.float32, shape=(None,D))以及后面的y都是对输入和输出做一个占位，代表将来我神经网络是用x作为输入，y为输出，shape要与你前面生成的数据集的形状一致，None代表不确定，因为一次喂入的数据量不确定，所以先写一个None，但是每个样本有几列是确定的，所以直接写出来。  

下面的h代表中间层w的维度，这个数字你可以随便确定，越高将来能达到的效果可能好一点，但是高的话会计算时间会很长，而w1的维度是[D,h],x的维度是[None,D],所以二者可以相乘得到一个[None,h]的矩阵，然后再经w2最后变成了一个[None,C]的矩阵，这个矩阵刚好和y是一样的，在中间层y1的地方要加一个激活函数，就是那个tf.nn.relu,而最后一层不需要。  

这个图模型大概就是下面这个样子：  

![](https://i.imgur.com/Uvha9ZS.png)

### 四、损失函数以及一些参数设置  

	# 参数 也可以写在最上面
	STEPS = 2000
	BATCH_SIZE = 256
	# 学习率
	learning_rate = 1e-0
	
	# 数据损失
	data_loss = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(labels = y,logits =y_))
	# 正则损失
	reg_loss = tf.add_n(tf.get_collection("losses"))
	total_loss = data_loss+reg_loss
	
	# 定义训练方式 -梯度下降
	train_function = tf.train.GradientDescentOptimizer(learning_rate).minimize(total_loss)  

STEPS代表一会训练的次数，BATCH_SIZE = 256代表一次喂入数据量，这个以后就是你上面x，y占位时候的None，learning_rate是学习率，下面有两个损失，一个是数据上的损失，这里使用的是交叉熵的方式（一定要求平均，就是reduce_mean，要不然计算的是一批数据所有的损失值），一个是对权重的正则化，train_function则定义一会使用梯度下降的方式找到最优值。  

### 五、开始训练  

	# 开始训练
	with tf.Session() as sess:
	    # 把上面定义的图模型全部初始化
	    init_op = tf.global_variables_initializer()
	    sess.run(init_op)
	    for i in range(STEPS):
	        start = (i*BATCH_SIZE)%K
	        end = start+BATCH_SIZE
	        sess.run(train_function,feed_dict={x:X[start:end],y:Y[start:end]})
	        if i%100==0:
	#             # 计算当前数据损失值
	            loss_v = sess.run(data_loss,feed_dict={x:X,y:Y})
	            print("this is %dth step,the loss is %f"%(i,loss_v))  


开始是把整个图模型都初始化一下，然后下面迭代STEPS次，每次喂入一个BATCH_SIZE的量的数据，sess.run就是开始训练了（sesss.run可以做很多操作，一般都是第一个参数传入计算方法，后面传入数据就可以了），下面是没经过100次输出一下损失值，也是使用run进行计算，前面输入计算方式，后面输入数据。  

### 画图或者保存模型  

一般迭代后，计算出来w与b之后就可以把模型保存下来，但是我们这次数据集比较小，所以迭代一次花费时间少，所以就直接把图画出来看下效果。  

	# 画出图像（画等高线，分类）
	    h = 0.02
	    x_min, x_max = X[:, 0].min() - 1, X[:, 0].max() + 1
	    y_min, y_max = X[:, 1].min() - 1, X[:, 1].max() + 1
	    xx, yy = np.meshgrid(np.arange(x_min, x_max, h),
	                         np.arange(y_min, y_max, h))
	    
	    
	    grid = np.c_[xx.ravel(),yy.ravel()]
	    # 将网格中的点喂入神经网络(训练过的)
	    # x是吧坐标系的点都喂入，然后按照计算出的图模型中的w计算y_ 得出得分值
	    probs = sess.run(y_,feed_dict ={x:grid})
	    
	    probs = np.argmax(probs, axis=1)
	    probs = probs.reshape(xx.shape)
	   
	    plt.contourf(xx, yy, probs, cmap="rainbow", alpha=0.8)
	    plt.scatter(X[:, 0], X[:, 1],c = Y_c,cmap = "rainbow")
	    plt.xlim(xx.min(), xx.max())
	    plt.ylim(yy.min(), yy.max())
	    plt.show()    


这里画的是等高线，把坐标点以0.02分成多份，然后组成grid这样的格子点，最后喂入你的神经网络，计算出得分值，然后处理一下把每个点都放到相应的分类里面，最后画出等高线，同样的值为一个颜色，然后在画出散点，最后显示。  

![](https://i.imgur.com/ZFSVL2K.png)

#### 六、总结 

![这里写图片描述](https://img-blog.csdn.net/20180808115138284?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 参考  

> https://blog.csdn.net/daxiaofan/article/details/70156357


	
	    
	    
	
	
	

