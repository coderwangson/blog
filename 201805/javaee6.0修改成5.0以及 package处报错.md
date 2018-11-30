## javaee6.0修改成5.0以及 package处报错
这个只是一个小技巧，至于为什么改，这就像修改jdk一样，肯定是不兼容咯。
修改方法见截图  

1. 首先建立一个javaee5.0的空web工程。
 
![](https://i.imgur.com/uAbWCoc.jpg)  
2. 


![](https://i.imgur.com/NrUvPzw.jpg)
3. 点击edit，拷贝那一行文字
   ![](https://i.imgur.com/ah76Lzj.jpg)
4. 同样的方法打开那个基于6.0的项目的buildpath，然后edit，也到那一行文字上，然后把刚才拷贝的一行文字粘贴上去就ok了。

![](https://i.imgur.com/xzoKibZ.jpg)

然后确认就行了。

----
### package处报错
出现这个问题原因蛮多，这里给出常见两个。
1. 也是修改buildpath，不过变成了修改jre。
   ![](https://i.imgur.com/nmwhGld.jpg)



![](https://i.imgur.com/sTAZms1.jpg)