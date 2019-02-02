# python中的小坑及安装包时问题小汇总

标签： `python`

---

### 1.python中二维数组：   

二维数组我之前一维使用list然后*某个数字，这样就行了，比如：

    a = [[1]*3]*2  
    
从输出结果看确实没什么问题：`[[1, 1, 1], [1, 1, 1]]`   

但是当你修改其中某个值的时候，比如`a[0][1] = 4`，你会发现最后a变成了`[[1, 4, 1], [1, 4, 1]]`，所以这种声明方法有问题。为什么出现这个问题，是因为[]*2这种拷贝是浅拷贝，拷贝完大家指向的还是同一块区域。详细原理在这个地方就不分析了，可以参考[pyton标准库][2]，如果在运行包含二维数组的程序时出现了和预期不符的问题，可以先从这个方向考虑一下，免得到处找bug。  

那么我们如果想声明二维数组该怎么做呢，可以直接或者间接声明如下：  

方法1 直接定义

    a = [[0, 0, 0], [0, 0, 0], [0, 0, 0]]

方法2 间接定义

    a = [[0 for i in range(3)] for i in range(3)]  
    
[参考][3]   

### 2.python中reshape  

reshape是numpy中的一个函数，用起来很好用，但是你要是一不注意可能就用错了地方，主要是在神经网络中你的数据格式可能和程序中的不一样，所以你可能想要改变下形状，比如你的数据是`[[1,2,3],[4,5,6]]``2行3列`，其中2代表特征个数，而3代表样本个数,所以上面数据代表3个样本每个样本特征分别为`(1,4),(2,5),(3,6)`但是程序要求数据要是3行2列，也就是行代表样本个数，列为特征，所以应该变为`[[1,4],[2,5],[3,6]]`,但是如果你用reshape的话原始数据reshape为3,2,那么数据变成了`[1,2][3,4],[5,6]`,所以和预期不符，这里应该用转置，所以当你神经网络各个地方都没问题，你可以考虑下是否数据转换的时候行列出了问题。   

### 安装python 3.5  

    conda create -n py35 python=3.5 anaconda

[参考][4]  

### pip安装时速度慢  

修改下载源为阿里的，[参考][5]  

### 安装tensorflow出现问题  

尝试一下如下命令: `pip install tensorflow==1.5` [参考][6]  

###  import tensorflow时出现Cannot uninstall 'html5lib'. It is a distutils   

Cannot uninstall 'html5lib'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.  

删除 `%pythonpath%\Lib\site-packages`下的`html5lib-0.999-py3.5.egg-info`.

[参考][7]   

### 安装opencv   

去[lfd][8]下载whl包，记着版本对应即可。[参考][9]  

### ImportError: numpy.core.multiarray failed to import  

[参考][10]  

### 安装caffe   

建议python安装3.5版本，要不然不好安装，官网方法[参考][11]，但是下载资源是下载不了，所以[参考][12]，要花一个积分下载资源，依赖包我没有安装，但是也没问题，可能conda安装py35的时候很多包都装的原因，如果不想花积分可以去我的github上下载[我的github地址][13]

### 安装pytorch  

[参考][14]，记得安装前面方法修改源，要不然安装速度慢，甚至安装不上去。   

### 直接安装全部的深度学习环境 （扩展知识）  

使用docker中有一个叫做deepo的，[参考][15]


  [1]: https://docs.python.org/2/library/stdtypes.html#sequence-types-str-unicode-list-tuple-bytearray-buffer-xrange
  [2]: https://docs.python.org/2/library/stdtypes.html#sequence-types-str-unicode-list-tuple-bytearray-buffer-xrange
  [3]: https://www.cnblogs.com/woshare/p/5823303.html
  [4]: https://blog.csdn.net/rona1/article/details/81776913
  [5]: https://blog.csdn.net/ipaomi/article/details/78466321
  [6]: https://blog.csdn.net/qq_28888837/article/details/80757731
  [7]: https://blog.csdn.net/qq_20373723/article/details/80323344
  [8]: https://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv
  [9]: https://blog.csdn.net/qq_28888837/article/details/80757731
  [10]: https://blog.csdn.net/llh_1178/article/details/81671348
  [11]: https://blog.csdn.net/qq_36735489/article/details/80141340
  [12]: https://blog.csdn.net/daixiangzi/article/details/82940305
  [13]: https://github.com/coderwangson/pythonwhl
  [14]: https://blog.csdn.net/sunqiande88/article/details/80085569
  [15]: https://blog.csdn.net/qq_36142114/article/details/81605372