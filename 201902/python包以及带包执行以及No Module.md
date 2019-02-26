# python包以及带包执行以及No Module

标签： `python`

---

python中包的概念个模块的概念和Java中类似的，模块为了把功能分离，包为了区别不同模块，一般包名唯一。  

但是我使用pycharm建立包的时候，在pycharm中一般毫无问题，但是在命令行使用python命令执行会报错。一直无果。最后就从头学了关于python包的概念，尝试不使用pycharm这类工具来进行python包的建立。

1、建立一个文件夹名字为Test，这个可以看做是个工程名
2、在Test下面建立一个包t（其实也是个文件夹）
3、在文件夹t下面建立文件\__init__.py,只有建立了这个文件t才能被python编译器认识，把其作为包来看待
4、在t下面建立两个文件t1.py,t2.py，如果在t1.py里面使用模块t2，其实直接import即可，因为在同一个包下面。
5、类似建立t我们在Test下面建立k文件夹，同样加入\__init__.py
6、在k下面建立k1.py

此时如果我们要在t1中使用k1，就必须带包了，我们在t1中加入
`from k import k1`，但是此时我们使用python编译就需要在Test这个文件夹下编译，我们在命令行输入`python -m t.t1`或者`python t/t1.py`这样就带包编译了。但是要注意如果你的文件中有关于路径的东西，其中的`.`代表你运行时候python编译器的位置。你可以在t1.py里面加入`print(os.path.abspath('.'))`进行验证，如果我们直接在t下面执行`python t1.py`（不要引入模块k，否则会报错）和在Test运行`python -m t.t1`输出是不一样的。  

但是注意在pycahrm中没有这个问题，因为pycahrm无论在哪里执行都可以运行，我也不知道为啥。

add(附加)：在python中也可以通过sys.path.append("")来加入你的包的位置，这样也能是python编译器找到你的包的位置，或者把你包的位置放入到环境变量中也行。如果在文件中或者再环境变量中改变了，那么你无论在哪里执行都可以。而如果只是有`__init__.py`那么要注意执行的位置，一定要在包的上层。

Tips：有了上面的介绍也就知道为什么别人的包如果你安装后会在python的lib里面，因为python的lib在环境变量里面有注册，所以避免每次都注册。而当你引入你自己工程下的包的时候，就无所谓了，你如果需要在命令行执行，只需要在工程下即可。  

参考：
https://blog.csdn.net/u013487140/article/details/79998465#fn:1  

https://blog.csdn.net/u013998480/article/details/78567346?utm_source=blogxgwz5







