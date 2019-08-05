# vscode配置python本地及远程开发环境

标签： `vscode` `python`

---

## 安装python和vscode  

python去官网下载安装（也可以安装conda等，但是要记住python对应的路径），安装vscode，同样去官网下载安装即可。  

## 安装插件   

安装完成vscode之后，点击下图那个地方，可以安装插件，


![vscode配置python本地及远程开发环境_1](https://wx2.sinaimg.cn/large/005Dd0fOly1g5e988o76kj309l07s3z7.jpg)

在搜索框里面搜索`python`,选择最高的那个即可。安装完python之后，就可以打开一个文件夹，进行运行了。点击菜单栏的File/Open Floder 就可以打开一个文件夹，在vscode里面，对每个文件夹都可以进行配置，配置文件在.vscode里面。  

## 配置python路径  

如下图  

![vscode配置python本地及远程开发环境_2](https://wx1.sinaimg.cn/large/005Dd0fOly1g5e9gwjq1sj301c06v0sq.jpg)  

点击那个小虫，则可以配置python环境，点击它，然后add configuation就可以添加一个python配置了：  

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "pythonPath": "${config:python.pythonPath}"
        }
    ]
}
```  
里面用到了pthonPath是引用系统的配置，所以点击系统的配置，也就是左下角的齿轮，并且切换成json格式。  

![vscode配置python本地及远程开发环境_3](https://ws3.sinaimg.cn/large/005Dd0fOly1g5e9l8bwwvj30it04rdgc.jpg)  

里面增加一行  

`"python.pythonPath": "C:/python/",`
ye'ke'yi
其中后面的`C:/python`代表你安装的pyton的路径，到此为止就全部配置完成了，之后可以点菜单栏的`debug/start debug`运行，也可以右键，`run python File in Terminal` .  

## 远程开发  

我们很多时候代码都在远程的服务器上面以及python也在远程，这个时候就需要远程开发了，vscode可以通过Remote SSH进行远程开发，Remote SSH本质就是在远程也装了一套vscode，这样就可以直接访问到远程的服务器，之后就相当于把远程的vscode在本地显示一下而已。所以python的配置按照本地的方式就可以了。  

首先安装Remote SSH ，也是在扩展里面安装，和安装pyton一样。安装完成后在左边会多出来一个图标。  

![vscode配置python本地及远程开发环境_4](https://ws3.sinaimg.cn/large/005Dd0fOly1g5e9z97crsj30n609kq4v.jpg)  

点击这个图片，然后点击设置，然后选择配置文件的位置，这里就用默认的C盘那个，之后就打开了一个配置文件。可以在里面填配置。  

    Host 自定义名字
    
    HostName 远程主机ip
    
    User 远程主机用户名
    
    IdentityFile 本地id_rsa路径  
    
    
剩下的关键就在于生成密钥，通过ssh命令生成，首先切换到.ssh目录里面，就是上面配置文件的上级目录。打开黑窗口，输入`ssh-keygen`，一路回车，就可以生成两个文件`id_rsa和id_rsa.pub`，之后把`id_rsa.pub`上传到你服务器的`.ssh`目录下。然后在远程主机中用`在远程主机用命令：cat id_rsa.pub >> authorized_keys把公钥加入到authorized_keys文件中`,之后可以尝试本地命令行输入`ssh username@remote_host_ip –i id_rsa`发现可以使用私钥链接即可，之后回到vscode 把上面的配置写好，就可以链接了。第一次连接的时候会在远程服务器里面下载一个vscode，链接成功后，左下角会有一个ssh的标记，说明你使用的就是远程的，剩下的可以直接安装python（是安装到远程服务器里面了），操作和本地一样，只是操作的资源是远程的。  

参考：  

https://zhuanlan.zhihu.com/p/67492389  

https://zhuanlan.zhihu.com/p/68577071



