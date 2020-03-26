# ftp 530 Non-anonymous session must use encryption   

问题描述： 在进行ftp/vscode里面的ftp-sync插件连接时出现如上`ftp 530 Non-anonymous session must use encryption `报错，并且使用FileZilla连接没有问题。   

经过分析，发现FileZilla快速连接里面的配置如下：  

![2020-02-19_155929.jpg](http://ww1.sinaimg.cn/large/005Dd0fOly1gc1rwhbhkxj309q08hgmz.jpg)  

可以看到加密的一项是 FTP over TLS，而使用ftp-sync则没有进行相关设置，所以进行设置即可。但是找了很多资料都没有找到，最后发现其实vscode里面的很多插件都是node.js写的，而ftp-sync也是用node.js写的，里面的实现是用的node.js的tls.connect,并且结合ftp-simple的文档 https://marketplace.visualstudio.com/items?itemName=humy2833.ftp-simple  ,在配置里面增加如下一项即可：  

```
"secure" : true,
"secureOptions": {"rejectUnauthorized":false}
```

