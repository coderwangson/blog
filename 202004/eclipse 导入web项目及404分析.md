# eclipse 导入web项目及404分析  

## 直接建立法  

当你从网上下载了一个web项目，然后想在你的电脑上运行，这就需要导入到eclipse里面了，最容易想到的方法就是先建立一个空白的dynamic web项目，然后将对应文件拷贝过来即可，包括.java、.jsp、一些配置文件和jar包。  

## 导入法  

这个可以基于上面建立法，你在建立之后，直接选中建立的这个项目，右键，然后Import->file system,然后选中下载的项目即可，这样eclipse就会自动帮你把文件放到对应的位置。  

## 404分析  

这里你要对项目稍微了解才能分析出来，我这里是根据我自己的情况分析了一下，首先看web.xml里面是否有一些限制，然后看对应位置的jsp或者servlet是否存在。  

其实我这些都有的，最后找了半天，才发现我下载的项目是用myeclipse写的，所以它的jsp这些文件是在webroot里面的，而WebContent是Eclips，而解决这个问题，你可以通过直接复制，或者参考其他的博文，将myeclipse项目用eclipse打开。这里我随便找了一篇，可以参考一下：https://blog.csdn.net/ahou2468/article/details/78970278

