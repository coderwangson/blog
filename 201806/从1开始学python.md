## 从1开始学python
life is short,you need python.
###0. 前言
之所以学习python，只是想把他作为一个工具，因为之后要学习机器学习之类的，要用到python中大量的模块，所以便想着学习一下python，但是因为只是当做一个工具用，所以也不需要说学的有多精通。这就像练功夫一样，你内功深厚了，即使用一根棍子也能发挥出强大的威力。  

但是在学习python之前，你需要有其他开发语言的基础，否则也不能叫做从1开始了。从1开始学习的好处是你可以触类旁通，随便一点就能理解，我记得我学Java写一个冒泡算法要学好久才能写的出来，但是用python我只需要学一下他的循环就ok了，立刻就能搞定。

###1. 输入输出
学习任何一门语言都从hello world开始，所以先学习一下输出函数，python中输出类似c语言。具体如下：  
	
	print("hello word")
**注意：** 结尾不需要分号，print后面没有f。

其输入函数如下：  
	
	input("请输入:")

**注意：**python中所有的输入都看做是str类型，所以如果你想把一个输入的内容变为其他类型，要进行强制转换。

###2. 基本数据类型
在python中的数据类型有str、int、float、bool、列表、字典、元组等。但是在python中声明一个变量的时候，不需要进行类型的指定，python会自动根据你赋的值进行判断，然后基于相对应的类型。对于一个变量来说，你可以通过type()函数来看其类型。如下:  
	
	```
	a=1
	b="hello"
	print(type(a))
	print(type(b))
	```
其会输出<class 'int'>、<class 'str'>，这就说明a是int类型，b是字符串类型。
###2.1 基本数据类型之间的转换
类似于c语言，可以通过int(),str()进行类型强制转换（但是前提要二者能相互转）。如下:  

	b="4"
	a=int(b)

这就把b转成了整型的a。

###3.分支结构与循环、代码书写缩进
分支结构就是if-else这种，循环就是while，但是注意python中没有for，但是有for-in，还有要注意的是python中代码书写，尤其是对于这种if-else，因为python中每个代码块不是通过{}来括起来的，而是通过代码缩进来控制的，所以初学者在这一点上可能迷惑，因为python的代码中不曾出现{}，并且python代码中的空格不能随便删，因为这种要求，所以python的代码看起来悦目一些。
#### 3.1 if-elif-else
第一个是分支结构，相当于c中if-else if-else，只是写法有点怪异如下：

![](https://i.imgur.com/mfZAr6r.png)

缩进很重要。

除了简单的if-else，你还可以写if-elif-else,其中el-if就相当于else if，只是换了种说法。如下：
	```

	if a and b:
	print("a and b is True")
	elif b:
	print("b is True")
	else:
	print("a is False")
	```

#### 3.2 循环结构
他的循环和c是一样的都是while的，但是由于python中没有for那种循环，所以可以使用while完成，如打印乘法表如下：
```

	i=1
	    while i<=a:
	        j=1
	        while j<=i:
	            print(str(j)+"*"+str(i)+"="+str(j*i),end="\t")
	            j+=1
	        print()
	        i+=1
```

同样注意的就是缩进，如果缩进不正确，他就会把不同行的代码误判为一个代码块，比如   print(str(j)+"*"+str(i)+"="+str(j*i),end="\t")和j+=1是属于一个代码块，但是如果缩进不正确，可能就会出错。