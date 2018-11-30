 - 当你编写了一个游戏或者一个工具类的话，你想分享别人用，这时候你就可以把他打包为jar分享给别人了。下面就告诉大家怎么打jar包，本教程通过myecplise来使用，ecplise类似。
 - 通过explore将其导出
 - ![这里写图片描述](http://img.blog.csdn.net/20170518212136623?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 - 导出的类型选择jar
 - ![这里写图片描述](http://img.blog.csdn.net/20170518212223530?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后选择jar文件位置，你可以在选择的时候建立一个jar文件即可
![这里写图片描述](http://img.blog.csdn.net/20170518212443751?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后再你刚才填写jarfile位置的位置就会出现一个.jar文件，这个时候就已经打包成功，但是如果对于游戏或者其他JFrame的运行程序，你会发现点击后并没有反应，这是因为程序找不到入口这个时候你需要通过winrar将这个jar打开，然后更改里面的清单文件，增加一个主函数所在的类即可，记得空格，类的完成名称，末尾回车
![这里写图片描述](http://img.blog.csdn.net/20170518213044415?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjg4ODg4Mzc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
改完保存一下就行了，不需要解压，也不需要重新打包。