virtualbox在安装一个系统的时候，硬盘位置其实可以自己定义的，但是由于我第一次安装，所以就安装到默认的位置了，所以安装到了C盘，造成我C盘红了，所以就想着迁移到别的盘符。   

首先找到那个vdi文件所在的位置你可以在设置中找到。

![](https://i.imgur.com/lGP7j2E.png)

然后把该vdi文件拷贝到你想要放置的位置。  

最后要设置一下使用命令

	VBoxManage internalcommands sethduuid "D:\VM\ubuntun.vdi"

运行该命令在黑窗口中运行，但是要记得黑窗口中盘符切换到你的VirtualBox所在的位置，这样VBoxManage命令才能使用。

该命令最后一个字符串代表你的新vdi文件位置  。

最最后，在VirtualBox中重新安装系统即可
![](https://i.imgur.com/QGiCgPc.png)


参考：
[修改VirtualBox虚拟机默认存储路径及虚拟机迁移方法](https://blog.csdn.net/superbfly/article/details/50843578)