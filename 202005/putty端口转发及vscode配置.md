# putty端口转发及vscode配置  

前几天遇到了一个问题：我有两台服务器A和B，服务器A可以直接利用putty通过ssh连接，但是B不可以，而B可以在A上面通过ssh连接，也就是我能够连接A，A能够连接B，但是我不能直接连接B，但是我现在想做的是我也要直接连接B，因为我想在vscode里面用远程连接B开发。下面给出我的解决方案。  

## step1   

在putty连接A后，开启端口转发功能，你可以在菜单栏->change settings->connection->SSH->Tunnel进行配置：  

putty端口转发及vscode配置_1.jpg  

注意端口要选几千以上，避免和一些软件端口冲突。在我们做完这一步之后，其实就对putty建立了一个监听，这样每当我们请求的上面这个端口，所有的流量就会走这个端口然后转发到A服务器，这样就相当于将A服务器作为跳板。下面我们需要做的就是在vscode里面配置一下，让我们连接B服务器的时候走这个端口即可。  

除此之外，给大家一个参考地址，里面有关于配置putty端口转发的详细教程，并且是让浏览器走的这个端口，这样我们浏览器上网的流量就可以通过A服务器转发走了。  

[https://blog.twofei.com/506/](https://blog.twofei.com/506/)

## step2  

下面我们需要做的就是让vscode走这个代理，其实ssh这个协议本身就是支持代理的，如下的命令就可以进行本地socket5代理：
```
ssh -D 58520 ipB -p 22
```  

其实ssh支持很多的代理方式，只是我们putty里面配置的就是socket v5，所以我们这里要用-D，其他的方式可以见我的参考的连接。  

然后我们在vscode的配置里面将这个-D写进去即可，在Remote Explorer->ssh targets->configures 里面配置如下即可建立SSH的动态端口转发:  

```
Host lab
    HostName 10.11.12.13
    User root
    DynamicForward localhost:58520
```  

其中lab可以随便写，HostName就是ip地址，user是用户名，下面DynamicForward相当于-D。
## 参考 

[https://zhuanlan.zhihu.com/p/57630633](https://zhuanlan.zhihu.com/p/57630633)
[https://www.jianshu.com/p/f6990f3a52eb](https://www.jianshu.com/p/f6990f3a52eb)

