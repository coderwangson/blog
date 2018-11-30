## python获取天气预报

### 1. 注册阿凡达数据  

这个网站上[（阿凡达数据）](http://www.avatardata.cn)给出了很多数据的接口，这样就省的你自己去爬，在这个网站上申请后，你在申请数据接口你会拿到一个appkey，这个appkey将来可以用来做一个接口参数。  

![](https://i.imgur.com/6CA8Bon.png)

你进入天气预报，然后申请数据，最终可以得到一个appkey。并且在里面还能看到接口的一些返回json信息。  

![](https://i.imgur.com/HOl7dYf.png)  

### 2. 使用python获取数据

#### 2.1 导入模块  

本次需要导入import requests。

#### 2.2 定义请求参数  

	url = 'http://api.avatardata.cn/Weather/Query'
    appKey = '*********'
    paramers = {'key':appKey, 'cityname':cityName}
    content = requests.get(url,params=paramers)
    jsContent = content.json()  

这段代码中url是固定的，appkey就是你自己的paakey，paramers中cityName就是你要查询天气的城市的名称。然后通过requestas获得了一个json串。  

#### 2.3 处理json  

上面能够得到了个json字符串，你只需要根据官网上的介绍吧字符串处理成你想要的格式就行了。  

比如你想冲json中拿到时间，那么你可以这样写：  

	date = jsContent['result']['weather']['date']  

这个书写格式你可以根据json进行处理。  

#### 2.4 获取天气预报代码  

	def getWeatherReport(cityName):
	    url = 'http://api.avatardata.cn/Weather/Query'
	    appKey = '*******'
	    paramers = {'key':appKey, 'cityname':cityName}
	    content = requests.get(url,params=paramers)
	    jsContent = content.json()
	    date = jsContent['result']['realtime']
	    jsWeather = jsContent['result']['weather'][0]
	    allContent = {}
	    allContent['AddressAndTime'] = "早安，^_^ \n今天是"+jsWeather['date'] + " " + u" 星期" + jsWeather['week']+"\n"
	    allContent['WeatherInfo']=processWeather(jsWeather['info']['day'])
	    jsLife = jsContent['result']['life']
	    allContent['LifeInfo'] = GetLifeInfo(jsLife['info'])+"\n"
	    return allContent['AddressAndTime']+allContent['WeatherInfo']+allContent['LifeInfo']
	
	# 把天气信息进行处理    
	def processWeather(weatherInfo):
	    tianqi = "今天天气 :"+ weatherInfo[1]+"\n"
	    wendu = weatherInfo[2]
		# 我这里把温度做了下处理把数字变成动态文字了
	    try:
	        wenduValue = int(wendu)
	        if(abs(wenduValue-20)<=2):
	            wendu ="真舒服"
	        elif(wenduValue-20>2):
	            wendu = "热"*((wenduValue-20)//3)
	        else:
	            wendu = "冷"*((20-wenduValue)//3)
	        wendu = "今天温度:"+wendu+"\n"
	    except:
	        wendu = "今天温度:"+wendu+"\n"
	    
	    fengli = weatherInfo[4]
	    # 异常抛出的妙用   因为有些风力不是3-4级这样类型，可能是微风这种，所以可能不能转为整数  
	    try:
	        fengli = int(fengli[0])
	        fengli ="今天的风力:"+"呼~"*fengli +"\n"
	    except:
	        fengli ="今天的风力:"+fengli +"\n"
	    return tianqi+wendu+fengli
	
	# 处理每日穿衣信息
	def GetLifeInfo(info):
	    iIndex = 0
	    LifeInfo = {}
	    LifeInfo['chuanyi'] = u"穿  衣: " + info['chuanyi'][1]
	    LifeInfo['yundong'] = u"运  动: " + info['yundong'][1]
	    LifeInfo['ziwaixian'] = u"紫外线: " + info['ziwaixian'][1]
	    LifeInfo['ganmao'] = u"感冒: " + info['ganmao'][1]
	    strContent = ""
	    for item in LifeInfo.values():
	        strContent = strContent + item + "\n"
	    return strContent  


### 3. 微信发送  

怎么使用微信聊天我在之前的一篇文章已经分享过了，所以不再累述。流程就是导入模块-->定义Bot-->寻找好友-->发送内容（内容就是上面天气预报处理后的一个字符串）。  

如果你想定时发送就做一个线程，这样每隔一天他就会自动发一次。

	bot = Bot()
	
	# linux执行登陆请调用下面的这句
	#bot = Bot(console_qr=1)
	def send_news():
	    print("begin----")
	    # 你朋友的微信名称，不是备注，也不是微信帐号。
	    try:
	        my_friend = bot.friends().search('**')[0]
	        try:
	            contents = getWeatherReport("**")
	            my_friend.send(contents)
	            bot.file_helper.send(contents)
	            ## 每86400秒（1天），发送1次，这里加了一个随机，避免每天都同时
	            print("end------")
	            t = Timer(86400+60*random.randint(-6,6), send_news)
	            t.start()
	        except:
	            contents = "早安"
	            my_friend.send(contents)
	            bot.file_helper.send(contents)
	    except Exception as e:
	         # 向文件传输助手发送消息
	        bot.file_helper.send('服务器异常出问题了' + str(e)) 


### 参考：  

[https://blog.csdn.net/qq_28888837/article/details/80395616](https://blog.csdn.net/qq_28888837/article/details/80395616)  

[https://blog.csdn.net/u011554976/article/details/80604004](https://blog.csdn.net/u011554976/article/details/80604004)  

[https://blog.csdn.net/u012581604/article/details/77371543](https://blog.csdn.net/u012581604/article/details/77371543)