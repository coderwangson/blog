## LaxTex入门

### 一.什么是LaxTex  

我只是根据这两天我自己使用的情况做一个总结，所以可能不全。首先要知道Tex其实就是一门语言，像Java或者C一样，只是他的作用是用于文字排版，你可以使用Tex语言写一个文件，然后用Tex的编译器编译一下，可以输出成pdf格式的文档。之所以使用Tex的原因是，他可以输入很多数学符号，这样你就不用在word里面点来点去了。  

### 二. 安装用于书写Lax的工具  

其实使用记事本就够了，但是我在这里使用的是MiKTeX，安装完成后，里面会带一个Texworks的软件，你可以使用这个软件进行书写文本，编译输出Pdf。软件下载地址:[https://miktex.org/download](https://miktex.org/download)。下载完直接进行安装即可。  

### 三. 编辑第一个HelloWord  

![](https://i.imgur.com/fKCiZkz.png)  

打开这个软件，然后就可以在里面书写Tex代码了。对于Tex同样有语法问题，这里不详细展开，你可以在文档里输入如下片段:  
	
	\documentclass{article}
	\begin{document}
	hello, world
	\end{document}  

然后如图所示：  

![](https://i.imgur.com/FXVM8o7.png)  

运行之后你就会看到一个pdf文件输出，里面显示了helloword，很简单吧。  

### 四.语法简单介绍  

1. \documentclass{article}
这个应该就是说整篇文档的样式，如果你有中文，你可以改为\documentclass{ctexart}  

2. \begin{document} 以及最后那个\end代表整个文档的开始以及结束。  

3. 如果你需要使用别的宏包（你可以想想为Java中的包），你只需要在\documentclass{article}标记下面加入\usepackage{包名}  


### 五.数学公式插入  

因为使用Tex主要好处就是可以插入数学公式，所以做下简单介绍。

##### 1. 插入公式  

使用$f(x)$这就是插入了一个函数f(x)，前面$与后面的$相当于界符，告诉编译器，这个地方是个数学公式，帮我解释一下。  

![](https://i.imgur.com/mzZQuXx.png)  

也可以用$$f(x)$$插入公式，只是会单独成行。  

##### 2. 上标下标  

	$a\_n a^3$   

其中_后面就是下标，^后面是上标。  

##### 3.极限  

	$\lim_{x \to +\infty}$  

\lim就是极限的意思会输出lim符号，而_说明后面是下标\to会输出那个箭头\infty是正无穷。其实在Tex中很多\xx的就是代表一些特殊字符，可以自行查阅学习。  

##### 4.关于极限那个下标在右下角而不在正下方解决方案  

如果使用$$ $$嵌入的不存在这个问题，否则你需要使用\mathop以及\limits 共同达到目的。  

![](https://i.imgur.com/FRXDdgY.png)  

这个可以把所有都放在符号正下方而不是右下角下标。

### 参考  


[LaTeX新人教程，30分钟从完全陌生到基本入门](http://www.latexstudio.net/archives/9377.html)  

[https://blog.csdn.net/chen_shiqiang/article/details/52101836](https://blog.csdn.net/chen_shiqiang/article/details/52101836)  

[TeX/LaTeX 常用宏包简介](https://blog.csdn.net/oney139/article/details/50442485)  

[如何latex中把下标放置到正下方](https://zhidao.baidu.com/question/873705252499505652.html)