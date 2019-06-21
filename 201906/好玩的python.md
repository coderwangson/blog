# 好玩的python

标签： `python` `fun`

---

## 做一个叫她起床的宠物  

利用的知识主要是使用爬虫或者一些网络接口获取数据，然后再使用`wxpy`把消息发送出去。这里做的是写了一个获取天气的，一个获取爱词霸的内容的。  

获取天气的使用`https://saweather.market.alicloudapi.com`这个提供的接口，我们向这个接口发送请求，会返回对应的json字符串，然后就可以对json字符串进行一些基本的转换，比如里面增加一些可爱的内容，课高度自定义，处理字符串在函数`process_content`里面。  

爱词霸则使用`http://open.iciba.com/dsapi/`这个接口，我们访问他，他就会自动返回一个json字符串，我们从中取出对应的中文和英文，然后使用`request`把图片下载下来。  

最后就是借助wxpy进行微信的登录以及指定发送时间（可用sleep控制）。  
代码可以去[github][1]上下载，注意不能直接使用，要修改微信号以及天气接口的appcode。    

## 分析一下你们的爱情大数据  

![image](http://wx3.sinaimg.cn/large/005Dd0fOly1g45nb249e4j30mo0dp436.jpg)  

去年过年在家没事情，想到之前网易云做的一个歌单的分析，所以想把我们的聊天记录用python分析一波，所以就分析了一下，然后网上下了一个展示的html，把分析结果展示了出来。不过里面用到的东西有点多，只能把技术栈大概分享一下。  

首先是要先把聊天记录导出来，这个需要是root过的安卓手机。  


1. 找到微信的数据库  

这个要求你的手机已经root，然后下载re文件管理器，找到/data/data/com.tencent.mm/MicroMsg这个文件夹，在下面找到一个文件名很长的文件夹。  

你可能会有两个名字很长的文件夹，那么你就找到有EnMicroMsg.db这个文件的那个，并且把该文件拷贝到电脑。这里存储的就是你的聊天记录。  

2. 导出聊天记录  

这里需要使用一个sqlcipher工具，这个工具可以打开上面的那个db文件，但是在打开之前你必须要先获取那个数据库的密码，数据库密码=md5(IMEI+uin)[0:7]，意思就是，将你手机的IMEI（或MEID码）和你微信的uin码加起来用md5加密后得到一串32位的字符串，取其前7位即为数据库的密码。  

1. IMEI（或者MEID获取）  
   你可以在手机上拨号界面输入* #06#*获取，输完之后你会拿到三个值，你一个个试一下，把他们都和uin加一下，有一个是正确的。
2. uin码  
   在路径/data/data/com.tencent.mm/shared_prefs下找到auth_infokey_prefs.xml文件并打开，找到如下文字，其中value后面跟着的就是你的uin码了。  
  ![](https://i.imgur.com/DURYQDf.png) 
3. 然后把二者拼接起来使用[md5加密](https://md5jiami.51240.com/)进行加密，并且去小写字母那个的前七位即可，这个就是密码。  

在得到密码后就可以用sqlcipher打开数据库，然后file-export把数据导出成csv或者txt格式的就行了。    

上面这么多工作其实只是把数据导出，之后有了csv文件，就可以分析你们的数据了，里面使用pandas进行读取csv文件并简单处理，比如时间戳变为时间，比如一些简单的统计。如果做词频统计，则可以使用jieba等。总之这个地方需要一些python技术才能解决。  

之后就是找一个html模板，把你分析好的数据以及图片进行展示即可。  

一些参考代码可以在[github][2]上下载。  



## 做一个打发时间的小机器人  

这个之前也有人说了，不过由于之前我在我博客里面写过[csdn][3]，所以就也死皮赖脸的再粘贴一遍。  

----
在准备工作做完之后，就可以进入正式开发了，其实这个程序主要是使用了图灵机器人的接口，你所有的回复内容是通过图灵机器人进行的，而我们所做的就是使用python调用相应函数就行了。
### 1. 安装wxpy
安装wxpy就相当于java中的安装一个jar一样，你从网上下载下来安装也可以，或者直接使用pip安装也行，这里推荐用pip安装，你也可以自行百度，反正能够安装上wxpy就行了，使用pip安装，则在命令行输入  
	
	pip install wxpy
### 2.申请图灵机器人
自行进入[http://www.tuling123.com/](http://www.tuling123.com/)这个网站进行注册，然后得到找到一个apikey：xxxxxxxxxx，这个号码以后再程序中需要使用。
###3. 程序书写
有了上面的两步，则基本的内容已经有了，现在只需要写代码，然后使用图灵机器人就行了。其中wxpy的作用就是进行微信的操作，比如微信的登录，以及与微信朋友的聊天等，而图灵机器人的作用主要是根据根据你的聊天内容输入（wxpy获得），然后产生一个聊天内容的输出。

#### 3.1 使用wxpy登陆微信
这个需要用到wxpy这个库，所以需要from wxpy import * ，然后使用bot = Bot(cache_path=True)，就可以进行登陆了（会弹出来一个二维码），那个参数用来设置可以缓存，这样不用每次登陆都扫码了。  
登录之后，你就可以通过bot来找到好友  

	found = bot.friends().search('xx')
其中xx代表好友备注。
#### 3.2 绑定聊天
在登录并且找到好友后就可以进行聊天了，这里用到了@bot.register()，这个就是把下面的函数绑定，然后如果微信好友发消息的话，就会调用该函数，所以你可以在该函数里面写相关的聊天代码。  

	@bot.register(found)
	def message(msg): 
	    ret = “你好”  
	    return ret

这个里面的found就是你上面查找到的好友，这里就是把他们绑定在一起了，之后就可以自动帮你回复了，ret就是回复的内容，而msg中就存储的是好友发送的内容。

**notice**：因为这个程序需要等待别人发送消息来，所以需要在后面加上embed()，相当于程序阻塞在那里，要不然程序一下就运行完了，就不能持续接发消息了。
#### 3.3 使用图灵机器人回复消息
上面虽然能回复消息了，但是回复的内容很单一，所以这里就需要使用图灵机器人进行内容处理了。在这里定义一个函数专门根据对方的msg来返回消息。  

	def reply(text):  
	    url = "http://www.tuling123.com/openapi/api"  
	    api_key="xxxxxxxx"  
	    payload={  
	        "key":api_key,  
	        "info":text,  
	        "userid":"666"  
	        }  
	    r = requests.post(url,data=json.dumps(payload))  
	    result = json.loads(r.content)  
	    if ('url' in result.keys()):  
	        return result["text"]+result["url"]  
	    else:  
	        return result["text"]  

该函数用到了json和requests所以记得import，功能就是用json进行数据传输，把你接收到的msg转换成一个回复。
有了这个函数，你上面的聊天代码就可以修改为  
	
	ret = reply(msg.text)
这样的话，他的回复内容就是图灵机器人所做的了，你就不需要关心了。

代码同样可以去github上面下载[github][4]，上面只是简单的一个自动回复，你可以做成指定对象的回复等。  

## 其实除了上面还有很多能做的，我还用深度学习做过一个用摄像头和手控制的flappy [github][5]，还做过一个简单的ocr识别软件（我女朋友到现在还在用）  

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g45npoxqqlj30gk05qmyd.jpg)  

可能python能做到的事情都是我们想不到的。  

最后，允许我打一波广告，码字不易   

欢迎大家关注我的微信公众号，未来上面会推送`python` `机器学习` `算法学习` `深度学习` `论文阅读` 以及偶尔的`小鸡汤`等内容。ようこそいらっしゃい！
![微信公众号](https://mmbiz.qpic.cn/mmbiz_jpg/jHLoMzblJGib3edEia7P3RicYib1HqcK5ItwKCibTW89mgx6KIbpgqQ2hJlWWbLuMhiclKZvjg1GD10HqIktoKEPo18g/0?wx_fmt=jpeg)




  [1]: https://github.com/coderwangson/Fun-python/tree/master/day_words
  [2]: https://github.com/coderwangson/Fun-python/tree/master/love_data
  [3]: https://blog.csdn.net/qq_28888837/article/details/80395616
  [4]: https://github.com/coderwangson/Fun-python/tree/master/tuling
  [5]: https://github.com/coderwangson/tensorflow_flappy