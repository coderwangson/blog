##python+qqbot实现qq聊天机器人
###1. 安装qqbot
使用pip安装qqbot 

	pip install qqbot
###2. 登录qq
在安装完qqbot后，就可以进行qq的登录了，使用qqbot命令进行登录，在命令行输入qqbot，然后会弹出来二维码，你进行扫码后便可以登录了，登录后就可以挂在那里不用管它，他就相当于一个qq客户端，用于处理qq。
###3.python编码
这个编码阶段和上一个微信的开发是类似的，都是通过一个方法接受消息，然后图灵机器人进行消息的处理并且给出回复的内容。只是qq的使用的 onQQMessage方法的做为消息回复的函数。  

	```
	
	#下面注册的响应函数的函数名必须为 “onQQMessage” ，函数参数也必须和上面的一致。
	def onQQMessage(bot, contact, member, content):
	      
	        ret = reply(content)
	        save = "收到消息: "+content+"\n"+"回复消息: "+ret+"\n"
	        save_to_file('D:\\content.txt', save)
	        bot.SendTo(contact,ret)
	
	```

我这里还做了一个把消息保存到文件，你可以不做，而reply函数和微信实现登陆是一样的参考我的微信登陆那个就行了。
###4.运行这个python
qq和微信不一样得还有就是，微信的这个python是直接运行的，而qq的这个python文件是交个qqbot运行的，具体方法就是把这个python文件拷贝到C:\Users\Administrator\.qqbot-tmp\plugins这个路径下，然后在开启一个命令行（**注意:是另外开启一个，不要在那个运行qqbot的黑窗口中输**），然后输入qq plug xxx其中xxx就是你的python的名字，这样就可以把py文件加载到qbot上咯。输入 qq unplug xxx就是从qbot上拿下。加载完py文件，你的qq就变成了一个智能qq咯。