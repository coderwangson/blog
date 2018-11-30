## ubuntu server安装桌面，window远程连接     

### 命令行操作远程linux  

因为我是腾讯云的服务器，所以在浏览器中登录觉得不舒服，所以就使用的xshell进行远程连接，不过xsehll没有桌面，所以可以用xshell做命令行处理。  

![在这里插入图片描述](https://img-blog.csdn.net/20180928200343917?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
  
主机就输入你的ip地址，然后连接的时候输入你的服务器的用户名和密码就能进入一个黑窗口里面了。  

###  ubuntu server安装桌面  

1. 获取root权限  
		
		sudo -i  
		
2. 安装gnome     

		$sudo apt-get  install gnome  
		
3. sudo apt-get install xinit

4. sudo apt-get install gdm  ( 登陆窗口，用于管理账户登陆的，还可以用来切换别的桌面环境。 )

5. sudo apt-get install ubuntu-desktop 

6. shutdown -r now (重启操作)  

在没有错误的完成了上面的安装后其实就已经安装了桌面环境了，因为我用的是腾讯云，所以在腾讯云登录服务器的时候，切换登录方式为vnc，这样你就进入一个桌面的系统。  

### window远程连接     

window桌面远程连接主要使用了xrdp技术，先安装如下内容。  

1. sudo apt-get install xrdp    
2. sudo apt-get install vnc4server  
可能你已经有了vnc这样的话体统会提醒你已经安装了。  

3. sudo apt-get install xubuntu-desktop  
4. echo "xfce4-session" >~/.xsession  
5. sudo service xrdp restart   

到了第五步你可以跳过下面的先远程连接试一下，因为我的做完第五步后远程连接会花屏，所以有加了一步。  

6. sudo vi /etc/xrdp/startwm.sh在. /etc/X11/Xsession前一行插入
xfce4-session  
7. sudo service xrdp restart      


如果还不行的

上面所有完成后就可以在windows里面进行远程连接了，计算机名写你远程服务器的ip，之后进入选择sesman-Xvnc"-->输入"用户名和密码"-->回车就行了。  


参考：
> https://blog.csdn.net/freezingxu/article/details/70767451
>https://blog.csdn.net/jdzwanghao/article/details/79875536  
>https://jingyan.baidu.com/album/8ebacdf0cdc64949f75cd555.html?picindex=9  
> https://blog.csdn.net/gaowu959/article/details/80458384   
> [参考了其中开启桌面共享部分，不知道有用没](http://www.mamicode.com/info-detail-1155821.html)




