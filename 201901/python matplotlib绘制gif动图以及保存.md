# python matplotlib绘制gif动图以及保存

标签： python matplotlib

---
谨以此文纪念我两天来的悲剧  

昨天我用lstm拟合sin曲线，看到别人画的做的动图很好看，并且还能保存下来，所以我也想做着玩一下，但是没想到在网上各种教程都不太对，最后还是无意间误打误撞成功了，所以纪念一下。
## matplotlib绘制动画  

### function 1.  

第一种方法就是采用matplotlib中的一种交互方式，这样我们就能保证在plt.show()的时候进程不会挂起，所以达到绘制图画的时候能够连续起来，这种在你做实验的时候画着比较简单，不需要新的知识，只要用过plt画图，几乎一看就明白，下面给出例子：  

``` python  

if __name__ == "__main__":
    fig = plt.figure()
    plt.ion()
    plt.show()
    ims = []
    for i in range(1,10):
        im = plt.plot(np.linspace(0, i,10), np.linspace(0, np.random.randint(i),10))
        ims.append(im)
        plt.draw()
        plt.pause(0.5)
        
```  

这段代码没什么实际意义，只是为了显示plt绘制动图，主要代码有`plt.ion()`代表着开启交互模式，所以`plt.show()`就不会影响后面的代码，后面就是普通的画图，画线段`plt.plot()`，然后`plt.draw()`就充当了绘制的作用，然后`plt.pause()`代表暂停0.5秒，这个由于比较简单，可以在网上找到大量例子，并且都是可以运行和实现的。   

或者像下面一样画到一个figure里面。  

```python 
import matplotlib.pyplot as plt
fig,ax=plt.subplots()
y1=[]
for i in range(50):
    y1.append(i)
    ax.cla()
    ax.bar(y1,label='test',height=y1,width=0.3)
    ax.legend()
    plt.pause(0.3)
```  

但是上面的都只能实现显示，但是保存不了，后来发现使用animation可以进行保存，所以就开始了animation求学之路。  

### function 2.  

其实使用animation画图也不是很麻烦，网上的大多数代码都能运行，并且也能看明白是在干什么。因为我觉得在这个过程中主要是在探索保存的过程中花了时间，所以就着重讲一下如何保存，先看下面这个例子。  

```python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
import numpy as np
if __name__ == "__main__":
    fig = plt.figure()
    ims = []
    for i in range(1,10):
        im = plt.plot(np.linspace(0, i,10), np.linspace(0, np.random.randint(i),10))
        ims.append(im)
    ani = animation.ArtistAnimation(fig, ims, interval=200, repeat_delay=1000)
    ani.save("test.gif",writer='pillow')
```  

其实即使不说，也能看的差不多懂，for循环主要是把你要画的内容存起来，存到ims中，然后使用animation在fig中华ims，其中间隔是200ms，网上还有其他的画的方式，比如使用一个方法更新，这里你可以自己搜索，一般网上的例子都是能用的，**但是网上的保存那个方法大多说都不能用** ，网上的建议是安装imagemagick或者ffmpeg，或者改一些路径等，但是我经过测试都不行，不过如果有ffmpeg之后可以保存成MP4没问题，可以参考[这篇文章][1]，但是即使安装了imagemagick并且writer使用的是imagemagick，也仍然不能保存gif，使用imagemagick的操作可以参考[这篇文章,主要是关于animation使用和 ImageMagick的安装等][2]和[这篇文章，关于修改配置文件][3]以及[这篇文章，也是修改配置文件][4]，但是经历过这一路坎坷，仍然是不起作用，最后保存的gif还是0kb，后来我在改变配置文件的时候，无疑中改错了，然后控制台出现了这个错误`MovieWriter imagemagick unavailable. Trying to use pillow instead.`，然后我就看到了了pillow这个单词，然后把writer改为pillow，emm，这个gif图片真是漂亮。 
![图1](1)

  [1]: https://segmentfault.com/q/1010000012078750
  [2]: https://juejin.im/post/5a7c4ab6f265da4e976e7feb
  [3]: https://own-search-and-study.xyz/2017/05/18/python%E3%81%AEmatplotlib%E3%81%A7gif%E3%82%A2%E3%83%8B%E3%83%A1%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B/
  [4]: http://www.hankcs.com/ml/using-matplotlib-and-imagemagick-to-realize-the-algorithm-visualization-and-gif-export.html  
  
  参考：  

      [1]: https://segmentfault.com/q/1010000012078750
      [2]: https://juejin.im/post/5a7c4ab6f265da4e976e7feb
      [3]: https://own-search-and-study.xyz/2017/05/18/python%E3%81%AEmatplotlib%E3%81%A7gif%E3%82%A2%E3%83%8B%E3%83%A1%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B/
      [4]: http://www.hankcs.com/ml/using-matplotlib-and-imagemagick-to-realize-the-algorithm-visualization-and-gif-export.html
  