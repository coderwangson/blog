 1. 关于maven maven是什么可以自行百度，我觉的就是一个下载jar包的，代码统一的工具。
maven的使用：
1.1首先要安装maven，可以去官网下载，是开源免费的。
1.2下载后解压，类似于tomcat一样，有很多配置文件，也有bin目录。
1.3下载解压后，也要配置环境变量，类似于java也配置一个maven_Home（不是必须）,然后配置path
1.4配置完环境变量后就可以在cmd黑窗口下进行使用输入mvn -v可以使用证明安装成功。
1.5对于个人使用到1.4就结束了，然后你就可以学习maven的使用了，网上大把教程[我的推荐](http://www.cnblogs.com/xdp-gacl/tag/Maven%E5%AD%A6%E4%B9%A0%E6%80%BB%E7%BB%93/)
 1.6对于工作的，一般要改配置文件![这里写图片描述](https://img-blog.csdn.net/20180421105447765?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
就是这个settings文件。主要添加两个位置
`<localRepository>F:\repository</localRepository>`增加这个，这个是说明你将来使用maven下载jar包下载到的位置。

```
<mirrors>
  <mirror>
    //该镜像的id
    <id>nexus-aliyun</id>
    //该镜像用来取代的远程仓库，central是中央仓库的id
    <mirrorOf>central</mirrorOf>
    <name>Nexus aliyun</name>
    //该镜像的仓库地址，公司会提供
    <url>http://*********</url>
  </mirror>
</mirrors>
```
这个是说明了你仓库的地址。
这都搞好后，你的maven就可以在cmd命令下使用了。然后需要做的是ecplise中安装插件，然后配置maven
![这里写图片描述](https://img-blog.csdn.net/20180421105949181?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![这里写图片描述](https://img-blog.csdn.net/20180421110047659?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

okmaven我的使用到此结束。

 2. redis的使用
 2.1 安装 也是开源的，所以下载解压就行了[安装教程](http://www.runoob.com/redis/redis-install.html)
 2.2安装完成后也可以配置环境变量，然后安装服务到win
 [安装服务](https://www.cnblogs.com/M-LittleBird/p/5902850.html)
 我在安装服务时候能安装上，但是在启动服务的时候总会出错，后来我发现先把那个带图案的![这里写图片就是这个](https://img-blog.csdn.net/20180421110740923?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 窗口关掉就行了。
2.3 然后就是修改密码，因为我们公司的项目链接redis的配置文件要密码，所以我这里也修改了redis的密码
[直接看第三条](https://www.cnblogs.com/springlight/p/6288902.html)
conf文件在这里
[你可以自行百度下载，因为csdn至少两分，我也没办法](https://download.csdn.net/download/qq_28888837/10364370)
