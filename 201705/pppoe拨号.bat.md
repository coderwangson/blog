现在很多的地方用pppoe拨号，但是有时候发现总是需要自己输入登录，很麻烦，（其实可以记住密码的但是很多人搞不好），所以这里给两种win10的pppoe拨号方法。
也作为保存，方便自己以后翻阅。

前期准备：
1.先打开网络共享中心（不会的话可以百度）
2.打开设置新的连接或网络
![这里写图片描述](http://img.blog.csdn.net/20170525131219972?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
3.下面默认，然后走到这一步，让选择类型
![这里写图片描述](http://img.blog.csdn.net/20170525131256269?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
选择pppoe

![这里写图片描述](http://img.blog.csdn.net/20170525131543226?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
然后填写完成后其实就可以了。此时其实就可以自动登陆了，每次你只需要连接一下就可以了。
![这里写图片描述](http://img.blog.csdn.net/20170525131704227?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


----------


----------
下面就是讲一下怎么更装逼的连接，如果你每次都要输入账号密码，这个方法同样适用。
![这里写图片描述](http://img.blog.csdn.net/20170525132119562?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
就是使用bat文件执行，bat里写的内容就是一个拨号的命令（但你要首先通过上上上面的方法建立了一个 图书馆 的pppoe），这样你一点击这个bat文件就会自动帮你连接了。

那一行命令的代码

```
@Rasdial 图书馆 username password
```

图书馆是自己建的那个pppoe的名字，username是用户名，password是密码。