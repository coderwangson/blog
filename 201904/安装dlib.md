# 安装dlib

标签： `python`

---  
## 直接安装法 

原来以为安装太麻烦，但是下面的方法确实可以使用，真的简单，所以转载做一个纪录。转载自[python 安装dlib][1]

<div class="htmledit_views" id="content_views">
                <p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">dlib是人脸识别比较有名的库，有c++、Python的接口。使用dlib可以大大简化开发，比如人脸识别，特征点检测之类的工作都可以很轻松实现。同时也有很多基于dlib开发的应用和开源库，比如face_recogintion库</span><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">等等。</span></p><p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">关于dlib的安装，如果直接运行pip install dlib，不出意外会发生错误。</span></p><p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">现在有以下解决方案：</span></p><p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">方案一：</span></p><p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">1、安装之前，升级pip版本，使用：<span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">python -m pip install --upgrade pip</span></span></p><p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;"><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">2、</span></span><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">下载dlib离线包，https://pypi.python.org/pypi/dlib/18.17.100#downloads</span></p><p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">3、使用pip install&nbsp;<span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">C:\Users\My_PC\Desktop\dlib-18.17.100-cp35-none-win_amd64.whl命令，便可以安装成功啦</span></span></p><p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;"><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">方案二：</span></span></p><p><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;"><span style="font-family:'-apple-system', 'SF UI Text', Arial, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif, SimHei, SimSun;">安装anaconda，直接使用命令 conda install -c menpo dlib=18.18 便可以轻松实现。</span></span></p>            </div> 
                
## 编译法  

因为那个似乎只有某些版本的，但是我需要的是19.17，所以矛盾，我是参考[这篇文章][2]按照源码安装的，首先通过`git clone https://github.com/davisking/dlib.git`下载源码，然后通过  

    cd dlib
    mkdir build; cd build; cmake ..; cmake --build .  
    
构建源码，最后安装到python。   

    cd ..
    python3 setup.py install  
    
但是  

如果生活这么简单就好了，因为这个构建编译需要使用gcc和g++,所以要保证两个版本都好，我这里都改成了`4.8`，而在这个过程中知道了如何不修改系统配置文件来直接修改这两个工具的版本，可以直接在命令行输入：  

    export CC=/usr/local/bin/gcc
    export CXX=/usr/local/bin/g++    
    
`/usr/local/bin/gcc`对应的是你的gcc的位置，如果你的bin下没有gcc，要么你安装一个，要么去/usr/bin下看一下，说不定有惊喜，总之，这两句话相当于临时修改了环境变量，只对于你当前的shell有效，所以不会影响到其他用户。修改完后，按照之前的方法编译成功。 

## 清华镜像法   

把你的pip的镜像源改为清华源，清华的镜像里面是有dlib的，不过如果是编译报错的话，同样按照上面的编译法，把gcc配置一下。
    
    




  [1]: https://blog.csdn.net/suyingiiie234/article/details/79653948
  [2]: https://gist.github.com/ageitgey/629d75c1baac34dfa5ca2a1928a7aeaf
