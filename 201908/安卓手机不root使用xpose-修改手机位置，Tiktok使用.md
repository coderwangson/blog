# 安卓手机不root使用xpose-修改手机位置，Tiktok使用

标签： `安卓` `xpose`

---
## 免root安装xpose  

xpose是安卓手机中很厉害的一个app，利用它可以实现手机的各种diy，但是由于xpose需要root，并且可能会造成系统的略微不稳定，所以希望能通过非root的方案来使用xpose，这里推荐一款app `VirtualXposed` 可以在 https://github.com/android-hacker/VirtualXposed/releases 点击下载，其本质就是一个虚拟的环境，安装完成后里面会自动装一个xpose，这些都是相当于运行在一个沙盒里面，所以不需要担心对手机有害。安装完成之后打开，是一个抽屉的结构如下：  

![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g5osmh7sc2j308c0etadq.jpg)  

上滑则能查看当前环境下安装的软件，可以看到安装了一个xpose，点击那个带点的园圈可以进入设置，比如重启当前环境（一般在安装了新的xpose模块后需要重启）  

## 修改手机信息  

这个可以通过一个xpose 模块来实现，应用变量，通过这个xpose模块则可以修改一些手机的配置信息，比如手机卡的位置，手机的型号等。可以在 https://www.coolapk.com/apk/com.sollyu.xposed.hook.model 下载。  

下载之后注意不是安装到自己的手机里面，而是先打开上面的VirtualXposed，然后在VirtualXposed里面安装。  

打开VirtualXposed，点击带六个点的园圈，点击添加应用，可以把本机已经安装的应用添加进去，也可以从内部存储里面找apk安装。  

安装的时候会有三个选项，那是安装方式，这里通过第一种方式安装。  

安装完成后，在xpose里面打开这个模块：  

打开xpose，点击左上角的三道杠，点击模块，然后勾选应用变量即可。  

到此为止，虚拟环境里面就能使用xpose的模块了。  

## tiktok  

tiktok是国外的抖音，它会自动识别手机卡，导致国内无法使用，或者通过拔卡的方式使用，而这里则把tiktok安装到上面那个虚拟环境里面，然后通过应用变量来修改手机信息达到绕过tiktok的检测。  

首先下载tiktok，文末给出下载方式，下载完成后，类似于安装应用变量，在虚拟环境里面安装。  

安装完成后，打开应用变量，点击tiktok来配置手机信息，这里主要是修改sim国际这个信息，修改为`JP`或者`US`等，你想切换哪个国家，就改为哪个国家的国家代码即可。修改完成后点击右下角加号，保存配置即可。然后重启VirtualXposed，打开tiktok即可。  

![安卓手机不root使用xpose-修改手机位置，Tiktok使用_1](http://ws2.sinaimg.cn/large/005Dd0fOly1g5oteu1i6vj30a40hcn1h.jpg)

------------------------
上面的方案主要在安装VirtualXposed上面，如果你的手机已经root并且安装过xpose了，其实就直接安装应用变量以及tiktok就可以了，在你手机的xpose模块里面启动应用变量就可以。  

欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！  
 
搜索 coderwangson 关注

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g48r27k7trj307607674r.jpg)  

回复tiktok获取下载链接  

其他方案请见：  https://www.mingjinglu.com/tool/296.html?replyTo=504 






