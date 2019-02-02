# linux搭建一个深度学习环境-包含tensorflow、pytorch，opencv，caffe，以及远程使用jupyter后台运行，编辑运行代码和使用jupyter切换conda环境  

标签： `python` `linux`

---  

## 安装tensorflow、pytorch、cv  

这三个模块都可以直接使用pip或者conda安装，cv也可以使用pip安装已经下载好的包安装.tar.gz可以在[这里下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/linux-64/)，这样在版本一致的情况下会更好安装，因为别人已经编译好了，而pytorch你可以在官网上看到有专门的命令，通过相应的命令使用pip安装即可。   

tensorflow参考[tensorflow][1]，pytorch[pytorch][2]

并且这三个都可以在python3.5下互相不干扰的运行。  

## 安装caffe  

py3.5的caffe我弄了半天实在是弄不好，最后就拷贝了实验室其他人编译好的python2.7版本的，但是在此页记录一下。

都知道，安装这些包可以通过直接conda的方法或者pip的方法一般都比较好弄，但是caffe通过conda虽然能安装，但是总是出问题，尤其是在py3.5上面，所以都以失败告终。  

除此之外还可以使用编译源代码的方法进行操作，但是python3.5编译的时候也是有一大堆问题，所以综上所述，**如果你想不花费太多时间，并且想使用caffe，那么听我的，用py2.7吧**。  

然后就是在编译的时候，可以直接看官网，其实主要就是相关依赖下载以及对应那个make.config文件的修改，并且在编译的时候可能会缺少很多依赖，所以太麻烦了，，[官网][3]。其实编译主要是使用make命令，然后对源文件编译，一般出现问题都是缺少依赖或者是配置写错了，但是这个一旦出现问题就不好定位，所以处理起来非常棘手。只能说慢慢来吧。  

除此之外caffe如果你们实验室有一个编译好的，那么你其实是可以直接拷贝过来用的，把他们编译好的所有文件拷贝到你的家里面，注意的是如果使用的话要把python版本修改一致。   

如何能让你的代码找到caffe，可以使用下面两种方法：
1、
```python
improt sys
#sys.path为一个列表，用什么方法加入都好啊，我用insert直接插到首位
sys.path.insert(0, ‘caffe_python的路径，我的为home/userw/down/caffe/python/')
import cafee
```
2、  

直接把caffe的路径添加到环境变量里面export PYTHONPATH=/path/to/caffe/python:$PYTHONPATH 写到 .bashrc中。
参考[让你的python找到caffe][4]。   

在使用别人的编译好的caffe时候，一定要注意用同版本的python，别人使用的是2.7编译的，那么你在使用也要用2.7版本的python。  

如果出现`No module named caffe`的错误，那么就是你的位置写错了。  

如果出现一串乱七八糟的错误，这就是版本的问题了，你可以尝试更换python版本或者一些模块的版本。   

## 远程连接jupyter以及使用jupyter切换conda环境    

在经历了上面魔鬼一样的折磨后，你终于能在服务器上使用这些模块了，但是每次敲完代码还要登录服务器，再上传太麻烦了，所以就想着使用jupyter直接进行代码编辑，上传功能。如果安装过conda，那么默认是有jupyter的。关键是怎么让jupyter能够远程使用。  

1.打开python输入以下语句  

```python
from notebook.auth import passwd
passwd()
# 然后按照操作输入密码（这个密码是你以后登录notebook时使用的密码）
# 输入之后就会得到一串字符，要记住这个字符，后面会用到
```

2.生成配置文件  

`jupyter notebook --generate-config`  
该命令会在用户的主目录下创建一个.jupyter文件夹，并在文件夹下生成jupyter_notebook_config.py文件
3.修改配置文件  

打开jupyter_notebook_config.py，可以使用如下命令找到该文件
```
find | grep jupyter_notebook_config.py
vim ./.jupyter/jupyter_notebook_config.py
```

然后修改如下 
```
c.NotebookApp.ip = '*'
#设置可访问的ip为任意。
c.NotebookApp.open_browser = False
#设置默认不打开浏览器
c.NotebookApp.password = '第1步生成的密文如shal:1e34r3424'
c.NotebookApp.port = 8888
c.NotebookApp.notebook_dir = '/home/'
```

4.启动jupyter  

`nohup jupyter notebook >/home/use/log/jupyter.log 2>&1 &`

其中nohub的作用就是让其能够后台运行，这样你关闭了ssh，jupyter仍然能使用，而/home/use/log/jupyter.log代表其运行的日志都输入到这个文件中。然后下面输出的一行代表进程号，你可以使用ps -ux看你的进程的情况。  

启动目录，如果你发现你在本地的目录不对，那么你想要哪个目录，你就可以选择在那个目录下启动jupyter。

5.这样你在本地电脑上输入http://localhost:1234以及密码就可以使用了。  
参考[Jupyter Notebook 远程连接及配置方法说明][5] [本地浏览器远程连接Linux服务器的jupyter-notebook][6]
虽然上面能够使用，但是其kernal只能选默认的，但是我有时候需要使用py3.5环境，有时候需要使用py2.7的环境，所以这个时候安装一个nb_conda，这样你的jupyter切换kernal的时候就能随意更换。    

`conda install nb_conda`   

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190202162141686.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_16,color_FFFFFF,t_70)

[如何在Jupyter Notebook中使用Python虚拟环境？][7]。


  [1]: https://blog.csdn.net/qq_28888837/article/details/80757731
  [2]: https://pytorch.org/get-started/locally/
  [3]: http://caffe.berkeleyvision.org/installation.html
  [4]: https://www.cnblogs.com/yinheyi/p/6062488.html
  [5]: https://www.jianshu.com/p/08f276d48669
  [6]: https://blog.csdn.net/u011253734/article/details/69525690
  [7]: https://www.jianshu.com/p/afea092dda1d  
  
 参考：  
      [1]: https://blog.csdn.net/qq_28888837/article/details/80757731
      [2]: https://pytorch.org/get-started/locally/
      [3]: http://caffe.berkeleyvision.org/installation.html
      [4]: https://www.cnblogs.com/yinheyi/p/6062488.html
      [5]: https://www.jianshu.com/p/08f276d48669
      [6]: https://blog.csdn.net/u011253734/article/details/69525690
      [7]: https://www.jianshu.com/p/afea092dda1d  
     
