##  根据tensor的名字获取变量的值     

#### 需求：   

有时候使用slim这种封装好的工具，或者是在做滑动平均时，系统会帮你自动建立一些变量，但是这些变量只有名字，而没有显式的变量名，所以这个时候我们需要使用那个名字来获取其对应的值。 如下：   

	input = np.random.randn(4,3)
	net = slim.fully_connected(input,2,weights_initializer=tf.ones_initializer(dtype = tf.float32))

这段代码看似简单，但其实帮你生成了一个w和一个b。如果你运行下面代码：  

	with tf.Session() as sess:
		    sess.run(tf.global_variables_initializer())
		    for v in tf.global_variables():
		        print(v)     
你会发现里面还有  

	
	<tf.Variable 'fully_connected/weights:0' shape=(3, 2) dtype=float64_ref>
	
	<tf.Variable 'fully_connected/biases:0' shape=(2,) dtype=float64_ref>    
	
这样两个变量，但是由于没有显式声明，所以我们要从其名字取值。
  
####  解决方案：     

1、从图里面取值：   

	print(sess.run(tf.get_default_graph().get_tensor_by_name("fully_connected/weights:0")))   
这个就是先拿到图，然后从图里面取变量 。   

2、直接取值  

	print(sess.run("fully_connected/weights:0"))    

直接把名字传进run里面就可以直接运行了，但是这个仍然拿不到变量，这个只能拿到变量值。  

####  参考：

> https://stackoverflow.com/questions/36612512/tensorflow-how-to-get-a-tensor-by-name    
> https://blog.csdn.net/silent56_th/article/details/75577320


