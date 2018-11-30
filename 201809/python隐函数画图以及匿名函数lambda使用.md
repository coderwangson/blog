## python隐函数画图以及匿名函数lambda使用  

### 一.匿名函数lambda使用   

因为一会画隐函数画图想用一下lambda匿名函数，所以就在这里学习一下其用法，本质上来讲lambda就是把函数换了中说法，其应用场景可以用在一些简单函数的定义上，比如你想定义一个比较大小的函数，而该函数就一句话，所以你不想很麻烦的使用def定义，此时就可以用lambda进行定义。用法如下：  

	comp = lambda a,b: a>b
	comp(3,2)   

这段代码的意思就是定义一个函数comp，只是是使用lambda定义的，一个函数要有参数和返回值，它的参数就是lambda后面的a,b而返回值就是冒号后面的a>b，这样本来要  

	def comp(a,b):
	    return a>b  

这样定义的函数就变成了一句话，所以lambda函数简单的使用就是这样。  

### 二.画隐函数    

在python开发中，很多时候要画函数图，对于显函数比较好画直接使用pyplot就行了，但是对于隐函数就有点麻烦，下面介绍两种隐函数画图的方式。  

#### 1. 使用Sympy库画隐函数  

使用方式如下：    

	%pylab inline
	from sympy.parsing.sympy_parser import parse_expr
	from sympy import plot_implicit
	exc = lambda exper: plot_implicit(parse_expr(exper))
	exc('x**2+(y-x**(2/3))**2-1')  

执行后是一个小心心：  

![1](img/python%E9%9A%90%E5%87%BD%E6%95%B0%E7%94%BB%E5%9B%BE%E4%BB%A5%E5%8F%8A%E5%8C%BF%E5%90%8D%E5%87%BD%E6%95%B0lambda%E4%BD%BF%E7%94%A8_1.jpg)   

这段代码意思很简单，%pylab inline是因为使用的Jupyter编译器，所以要加上，下面两个from是导入库，exc就是定义了一个函数，输入一个隐函数表达式，然后就能画出对应图形，$x^2+(y-x^{2/3})^22-1 = 0$就是一个心形线。 不过这段代码执行效率很低，要执行很久才能执行出来。  

#### 2. 使用Matplotlib结合等高线画隐函数   

	import matplotlib.pyplot as plt
	import numpy as np
	#作点
	x=np.linspace(-3,3,500)
	y=np.linspace(-3,3,500)
	
	#构造网格
	x,y=np.meshgrid(x,y)
	z = x**2+(y-x**(2/3))**2-1
	#绘制等高线,8表示等高线数量加1
	plt.contour(X,Y,z,0)
	plt.show()    

这个画圆还行，画心就只能画一半，不过其运行很快。代码的含义：  

x,y就是在坐标域画成网格线，然后定义一个z就相当于等高线函数，最后关键是contour中最后一个参数，你需要传入0，否则其会画出来很多线。具体含义我也不太清楚，你如果自己定义的话，只需要修改z就行了，把z等于你的隐函数就能画出一个隐函数图来。  

### 参考  

> [https://docs.sympy.org/dev/modules/plotting.html?highlight=sympy%20plotting%20plot#module-sympy.plotting.plot](https://docs.sympy.org/dev/modules/plotting.html?highlight=sympy%20plotting%20plot#module-sympy.plotting.plot)  
> [Python隐函数作图](http://doraemonzzz.com/2018/07/15/Python%E9%9A%90%E5%87%BD%E6%95%B0%E4%BD%9C%E5%9B%BE/)



