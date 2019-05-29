# visdom端口问题（全蓝，没内容）

标签： `pytorch`

---

## 安装visdom  

无论是使用`pip`或者`conda`都能安装的，网上的相关教程也很多，直接安装即可。可能会出现有些网上说的缺少相应`js`文件的问题，可以参考[这篇文章][1]去下载对应的js文件。  

![image](http://wx3.sinaimg.cn/large/005Dd0fOly1g3fx8ceagnj30lu0ez0ue.jpg)  

## 修改端口  

由于使用的是服务器，所以可能端口大家会相互冲突，所以这个时候需要修改端口，在启动`visdom`时候，使用` python -m visdom.server -p 2333`，这样就指定使用的是2333端口。并且能成功在浏览器里面打开，但是我的问题是我写的python代码运行过后，visdom并没用更新，后来才发现，我们在写python代码的时候对visdom实例化需要指定端口
```python
import visdom
vis = visdom.Visdom(port=2333)
```


  [1]: https://blog.csdn.net/chai_zheng/article/details/81545365