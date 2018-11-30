## Ubuntu安装、python安装以及Tensorflow安装

### Ubuntu安装

#### 1. 首先下载ubuntu的iso镜像，地址在[种子地址](http://releases.ubuntu.com/16.04/ubuntu-16.04.4-desktop-amd64.iso.torrent?_ga=2.177391276.82019049.1529457189-834487013.1529457189 "种子地址")。

#### 2. 安装virtualbox

#### 3. 在virtualbox中安装ubuntu
关于安装ubuntu，网上有一大堆教程，自行百度[安装教程](https://jingyan.baidu.com/article/7f766daff541cd4101e1d0cd.html)

#### 4.安装python

因为ubuntu是默认有python的，所以你只需要验证一下你的python是否能够使用即可。默认版本2.7.

#### 5. 安装anconda  

在ubuntu中安装anconda参照网上方案，这里强调的是关于anconda的作用，它其实可以看做是一个对python的管理，在你安装完anconda后，你就可以安装多个环境，并且每个环境都是互相不相关的。  

安装完anaconda后，你可以再开启一个终端输入conda list，如果显示没有该命令，就说明你的环境变量没有配置好，此时在终端中输入  
 
	sudo gedit ~/.bashrc
然后在末端输入  

	export PATH="/home/coder/anaconda3/bin:$PATH"

后面那个home/coder是你安装anaconda的位置，这个环境变量设置的功能好win上是类似的，都是为了能在所有终端下运行命令。  

在修改完环境变量后你就可以使用conda list命令了，然后就可以使用anaconda管理了。

小插曲--使用virtualBox时win和ubuntu不能双向粘贴解决方法[双向粘贴](https://jingyan.baidu.com/article/574c521917db806c8d9dc18c.html)
##### 5.1 建立一个tensorflow环境
1、先更改anconda下载镜像为清华仓库镜像（这样舒速度快一点）
打开Anaconda Prompt，输入清华仓库镜像，这样更新会快一些：

	conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
	conda config --set show_channel_urls yes
2、建立一个python环境，python版本为3.6，取名字为tensorflow  

	conda create -n tensorflow python=3.6

3、这样的话你就拥有了一个自己的环境，然后你激活这个环境就可以使用了，你可以在这个环境下安装一些python模块。  

	source activate tensorflow  

在win中不需要写source
![](https://i.imgur.com/L2dZOcW.png)

以后你就可以在这个环境下工作了，并且避免和他人冲突。

4、激活这个环境后，就可以在里面安装tensorflow，掌握这里使用的是pip方式安装的  

	pip install --upgrade --ignore-installed tensorflow

安装方式有很多中，任意选择一个即可。

5、然后进入python，使用import tensorflow进行测试，看是否安装成功

---------------------------------------------
2018年10月：  

之前我安装tf都没问题，这次安装忽然报错  
	
	import tensorflow failed, "ImportError: DLL load failed".   

所以就找了教程，发现把安装tf改成下面就行了：

	pip install tensorflow==1.5

参考文献：


>[双向粘贴](https://jingyan.baidu.com/article/574c521917db806c8d9dc18c.html)

>[简单过程](https://www.cnblogs.com/HongjianChen/p/8385547.html)

>[参考配置环境变量](https://www.2cto.com/kf/201612/571112.html)


>[详细过程（后悔没早点看到）](https://blog.csdn.net/luojie140/article/details/78696330 "详细过程")  

> https://github.com/tensorflow/tensorflow/issues/17393

