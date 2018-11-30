## python打包成exe  

### 前言  

python打包成python有两种方式pyinstaller 和py2exe，但是py2exe由于老旧，并且兼容性不好，所以不被采用，这里使用pyinstaller 进行打包。  

### 安装pyinstaller   

使用pip安装   

	pip install PyInstaller  

这里先给申明一个概念，我之前做的时候把PyInstaller  安装后不知道怎么办了，因为我以为这个也想以前安装py模块一样，要在python中使用，其实不然，其实你可以看做你是安装了一个exe程序，由于你使用pip安装，所以安装位置在python安装路径下的Scripts下，你可以找到一个pyinstaller.exe这样的文件，这个你就可以当做是一个应用程序，只是用来打包python成exe的。  

### 打包exe   

在命令行下输入   

	pyinstaller -w -F D:\python\test.py  
其中D:\python\test.py是你python文件所在位置，而-F，-W是一些打包参数，如果是GUI编程你可以加上-w，这样就不会闪现黑窗口了，如果你想打包成一个单独exe你可以加上-f，具体使用你可以自行参考官网说明。   


>https://www.crifan.com/use_pyinstaller_to_package_python_to_single_executable_exe/ 
>https://jingyan.baidu.com/article/a378c960b47034b3282830bb.html

### 中间遇见的问题   

如果一切都是那么顺风顺水当然好了，但是emmm，你在按照上面做法做完，发现还是不行。  

####   ‘str’ object has no attribute ‘items’、ModuleNotFoundError: No module named 'setuptools._vendor'   

这个是需要更新setuptools，使用pip install --upgrade setuptools 更新到最新版本后问题解决。  

> https://blog.csdn.net/qq_39360343/article/details/82772916  

#### Cannot find existing PyQt5 plugin directories  

找到 /Library/plugins路径下的PyQt5文件夹，将里面的dll动态库pyqt5qmlplugin.dll复制出来按照错误提示的路径，一个个的新建文件夹，形成目录  
C:\qt5b\qt_1524647842210\_h_env\Library\plugins，将刚才复制出来的dll动态库拷贝进去即可。  

>https://www.cnblogs.com/jkn1234/p/9672957.html   

#### 拒绝访问   

用管理员模式启动cmd即可   

####  failed to execute script ***  

这是因为你的py文件中使用了一些资源是相对路径，这个时候你只需要把你的资源拷贝到相对exe的位置即可。  

>https://blog.csdn.net/zuliang001/article/details/80762574


