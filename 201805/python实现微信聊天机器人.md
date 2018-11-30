## python实现微信聊天机器人
###具备基础：会编程就ok
###电脑环境：已经安装过python，在命令行输入python能成功就行
![](https://i.imgur.com/QKI2P0I.jpg)

----
在准备工作做完之后，就可以进入正式开发了，其实这个程序主要是使用了图灵机器人的接口，你所有的回复内容是通过图灵机器人进行的，而我们所做的就是使用python调用相应函数就行了。
###1. 安装wxpy
安装wxpy就相当于java中的安装一个jar一样，你从网上下载下来安装也可以，或者直接使用pip安装也行，这里推荐用pip安装，你也可以自行百度，反正能够安装上wxpy就行了，使用pip安装，则在命令行输入  
	
	pip install wxpy
###2.申请图灵机器人
自行进入[http://www.tuling123.com/](http://www.tuling123.com/)这个网站进行注册，然后得到找到一个apikey：xxxxxxxxxx，这个号码以后再程序中需要使用。
###3. 程序书写
有了上面的两步，则基本的内容已经有了，现在只需要写代码，然后使用图灵机器人就行了。其中wxpy的作用就是进行微信的操作，比如微信的登录，以及与微信朋友的聊天等，而图灵机器人的作用主要是根据根据你的聊天内容输入（wxpy获得），然后产生一个聊天内容的输出。

####3.1 使用wxpy登陆微信
这个需要用到wxpy这个库，所以需要from wxpy import * ，然后使用bot = Bot(cache_path=True)，就可以进行登陆了（会弹出来一个二维码），那个参数用来设置可以缓存，这样不用每次登陆都扫码了。  
登录之后，你就可以通过bot来找到好友  

	found = bot.friends().search('xx')
其中xx代表好友备注。
####3.2 绑定聊天
在登录并且找到好友后就可以进行聊天了，这里用到了@bot.register()，这个就是把下面的函数绑定，然后如果微信好友发消息的话，就会调用该函数，所以你可以在该函数里面写相关的聊天代码。  

	@bot.register(found)
	def message(msg): 
	    ret = “你好”  
	    return ret

这个里面的found就是你上面查找到的好友，这里就是把他们绑定在一起了，之后就可以自动帮你回复了，ret就是回复的内容，而msg中就存储的是好友发送的内容。

**notice**：因为这个程序需要等待别人发送消息来，所以需要在后面加上embed()，相当于程序阻塞在那里，要不然程序一下就运行完了，就不能持续接发消息了。
####3.3 使用图灵机器人回复消息
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

###4. 代码
```
	'''
	Created on 2018年5月18日
	@author: Administrator
	'''
	"#codeing=utf-8"
	import json  
	import requests  
	from wxpy import *  
	
	def reply(text):  
	    url = "http://www.tuling123.com/openapi/api"  
	    api_key="xx"  
	    payload={  
	        "key":api_key,  
	        "info":text,  
	        "userid":"666"  
	        }  
	    r = requests.post(url,data=json.dumps(payload))  
	    result = json.loads(r.content)  
	    if ('url' in result.keys()):  
	        return ""+result["text"]+result["url"]  
	    else:  
	        return ""+result["text"]  
	
	
	bot = Bot(cache_path=True) #登录缓存  
	print('小新上线')  
	found = bot.friends().search('xx')
	print(found)

	@bot.register(found)
	def message(msg): 
	    ret = reply(msg.text)  
	    return ret

	@bot.register(found)
	def forward_message(msg):  
	    ret = reply(msg.text)  
	    return ret 
	embed()

```