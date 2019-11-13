# pycharm显示远程图片

标签： `windows`

---

首先，你要知道pycharm可以通过ssh链接到远程服务器，并且也能够用pycharm运行远程服务器的代码。可以参考 https://www.xncoding.com/2016/05/26/python/pycharm-remote.html 这里配置  

## 远程图片显示问题   

如果上面的你都搞定了，但是发现，用opencv或者Image不能显示图片，那么就按照下面的步骤做即可。  

首先，开启服务器的ssh转发服务，这样当遇到有GUI的请求，就可以转发了。

```vim /etc/ssh/ssh_config```  

![2019-11-01_212931.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1g8iv1wny6oj306z0233yr.jpg) 

把这三个打开即可。  

此时说明远程转发开启了。  

其次，本地用ssh链接服务器，我用的`mobaxterm`,因为它里面自带的有`x-server`服务。  

![2019-11-01_213201.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1g8iv4e5v0xj30fg02awfl.jpg)  

如果你用的windows的黑窗口，则需要安装xming等`x-server`服务。  

此时你在mobaxterm里面尝试输入`xclock`就能弹出一个表，说明gui转发成功，然后输入`echo $DISPLAY`即可得到本地处理转发的位置，我的输出为`localhost:20.0`，说明`localhost:20.0`在处理gui，所以在pycharm配置一下即可。  

最后，打开pycharm的Run-->Edit config-->python-->xx.py  

修改Environment variables,增加`DISPLAY=localhost:20.0`这个变量即可。  

代码测试：

```
from matplotlib import pyplot as plt
import cv2
from PIL import Image
import numpy as np
img  =np.zeros((224,224,3))
plt.imshow(img)
# plt.show()
cv2.imshow("a",img)
cv2.waitKey()
```  

可以发现能够展示，注意的是，如果用的Image显示，一定在最后增加一个等待的代码，比如`input()`，否则会一闪而过。  


后记  

后来发现其实plt展示的方式，什么都不用配置，并且plt.imshow()即可以接受图片PIL，也可以接受数组，也挺方便。



