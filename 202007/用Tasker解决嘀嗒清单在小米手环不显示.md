# 用Tasker解决嘀嗒清单在小米手环不显示  

新买了小米手环5，里面有一个很好用的功能就是app显示，并且能够查看显示的具体内容。但是，用了一阵子发现嘀嗒清单的消息无论怎么设置都不显示，不知道哪里的问题，我看论坛也有很多人反应，可能是对这些记时的软件有这个屏蔽吧，但是我又是一个钻牛角尖的人，那怎么办呢，后来就想到了我之前用过的一个神器Tasker，这个工具其实就是能够根据你的手机的一些状态然后执行具体的任务。所以我们的目的是当手机接受到了嘀嗒清单的通知，就执行一个发出通知的任务（因为小米手环5可以对Tasker提醒）。其实这就是做了一个中转，我们将消息托管给Tasker，然后触发Tasker再发一个消息。  

## 具体流程  

下载Tasker，可以在酷安等平台下载。打开Tasker后会发现主页有三栏：配置文件、任务、场景。配置文件就是用来配置条件和一系列操作的（也就是监视手机的一些状态，比如通知栏有消息，电池充满等），任务就是用来配置一系列操作的（执行一些操作，比如发出通知，关闭蓝牙等）。场景一般用不到。

用Tasker解决嘀嗒清单在小米手环不显示_1.png

一般我们在Tasker里面制作一个功能，流程是现在任务里面创建一系列的操作：比如我们当前主要是想让Tasker发出通知。然后再配置文件里配置一些条件并且选择执行的任务。 

选择任务那一栏，点击加号你就可以新建一个任务，之后进入任务编辑界面。一般一个任务可以执行很多个动作，因为这里只需要一个提醒，所以点击加号，这里选择警报->通知，标题的话自己选，文字里面选通知标题即可。这样就做了一个能够提醒的任务，你可以点击下面的小三角运行一下看看效果。  

然后在配置文件里面新建一个触发事件：点击右下角的加号选择事件->界面->通知，所有者程序选择嘀嗒清单，搞完之后，点上面返回，就会弹窗让你选择任务，这里就选择你刚才新建的那个即可。

其实原理就是当嘀嗒清单有了通知，然后就会触发Tasker的事件，然后会执行一个任务，就是Tasker发出一个通知，通知的文字就是嘀嗒清单的通知内容。  

然后我们在小米运动里面的app通知里加入Tasker即可。  

通过上面的讲解，其实发现Tasker用作嘀嗒的通知实在大材小用，其实可以做一个充满电提醒，晚上提醒睡觉，将这些和小米手环结合，其实可玩性还是很高的。