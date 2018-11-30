## 使用python进行英语文献翻译

### 1. 前期准备  

安装PyPDF2模块、申请百度翻译api。其中模块安装大家可以根据情况，因为很多模块我之前安装过，比如requests，所以现在都是直接用的，如果你在使用的过程中，缺少了摸个模块，你可以自己进行安装。  

百度翻译api申请，你可以进入[百度翻译开放平台](http://api.fanyi.baidu.com/api/trans/product/index)进行注册申请，然后在开发者信息里面找到你的Appid以及秘钥。  

![](https://i.imgur.com/O3ZCAhr.png)  

这个秘钥后来进行翻译的时候回用的到。  

### 2. 处理pdf- 提取文字  

这个工作使用PyPDF2就可以搞定，主要使用的extractText函数进行pdf的文字抽取，具体代码如下：  

	from PyPDF2.pdf import PdfFileReader, PdfFileWriter, ContentStream
	## 处理pdf
	def getDataUsingPyPdf(filename):  
	    pdf = PdfFileReader(open(filename, "rb"))  
	    content = ""  
	    num = pdf.getNumPages()
	    for i in range(0, num):  
	        extractedText = pdf.getPage(i).extractText()
	        content +=  extractedText + "\n"  
	    return content  

其中filename就是你待处理pdf的位置，最终这个函数的返回值就是整个pdf的全文。  

### 3. 使用百度翻译 

百度翻译提供了接口供使用，你只需要把你的appid和秘钥拿到，然后传入正确的json串，百度翻译这个接口就会返回一个json串，你对这个json串进行分析就ok了。代码如下：  

	import requests
	import string
	import time
	import hashlib
	import json
	 
	#init
	api_url = "http://api.fanyi.baidu.com/api/trans/vip/translate"
	my_appid = "你的Id"
	cyber = "你的秘钥"

	 
	def translate(word):
	    #init salt and final_sign
	    salt = str(time.time())[:10]
	    final_sign = str(my_appid)+word+salt+cyber
	    final_sign = hashlib.md5(final_sign.encode("utf-8")).hexdigest()
	    #其中form和to可以区别en,zh构造请求参数
	    paramas = {
	        'q':word,
	        'from':'en',
	        'to':'zh',
	        'appid':'%s'%my_appid,
	        'salt':'%s'%salt,
	        'sign':'%s'%final_sign
	        }
	    my_url = api_url+'?appid='+str(my_appid)+'&q='+word+'&from='+'zh'+'&to='+'en'+'&salt='+salt+'&sign='+final_sign
	    response = requests.get(api_url,params = paramas).content
	    content = str(response,encoding = "utf-8")
	    json_reads = json.loads(content)
	    return json_reads['trans_result'][0]['dst']+" "  

这个函数功能就是传入一个英文字符串，然后返回一句中文。  

### 3. 使用上面两个函数，进行文本处理  

	content = getDataUsingPyPdf("08095313.pdf")
	clist = content.split(".")
	i=0
	while(i<clist.__len__()):
	    print(translate.translate(clist[i].replace("\n"," ")))
	    i+=1  

其中我按照 . 把英文文章进行了分句，这样就可以用百度翻译函数进行单句的翻译。在翻译的过程中我也进行了replace("\n"," ")，这样主要目的是有时候你的句子可能由于pdf转换，一句话被回车给隔开了，导致翻译的时候不准确。你也可以自己进行很多自定义处理，比如在读pdf的时候，把句子处理一下，都是可以的。我本次测试了效果还ok。下面贴出部分原文以及翻译后。  

原文：  

![](https://i.imgur.com/aC91tqZ.png)  

翻译后：  

![](https://i.imgur.com/YWrBUGs.png)

使用[迅捷pdf翻译](http://app.xunjiepdf.com/fanyi)：

![](https://i.imgur.com/Qf1Ieag.png)  

本次处理的结果是在控制台输出的，你可以重定向到txt，甚至写到pdf中。  

参考：  

[百度翻译api调用](https://blog.csdn.net/mr_guo_lei/article/details/78617669)

[pdf处理](https://blog.csdn.net/cheany/article/details/79109903)
