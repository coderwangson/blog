# python实现自动拨号

标签： `windows` `python`

---

## 缘起  

实验室有两台电脑，一台台式，一个笔记本，台式连着宽带，并且上面有一个小服务需要联网，因为实验室的网络不是很稳定，所以可能有时候网络会自动掉线，所以每次都需要自己重新手动拨号，所以不想每次都再打开台式的显示屏，所以就自己用python写了一个能够自动判断当前网络是否断开连接的程序，如果断开的话则自动重拨。  

而重拨由于使用的pppoe所以可以参考我的[另外一篇文章][1]，上面讲了怎么使用命令行拨号。然后只是把这个命令行换到python代码里面即可，然后一个`while True`即可。  

## python代码  

挺简单的，主要使用了os模块，直接执行三个命令，一个是上面我的pppoe拨号，一个是断开连接，还有一个是`ping`用来测试网络是否通了。  

```python  
import os
import time
while True:
    res = os.system('ping 8.8.8.8')
    # 没有网络的时候res为True
    if res:
        os.system('@Rasdial 宽带连接 /DISCONNECT') # 先断开宽带连接（这个宽带连接是你的网络名字，可以叫做别的）
        # 然后重新拨号
        os.system('@Rasdial 宽带连接 账号 密码')
    # 有网络 什么都不做
    else:
        pass
    # 每隔 5分钟进行一次检测
    time.sleep(5*60)

```



  [1]: https://blog.csdn.net/qq_28888837/article/details/72724554  
  
  参考：  
  https://blog.csdn.net/qq_28888837/article/details/72724554