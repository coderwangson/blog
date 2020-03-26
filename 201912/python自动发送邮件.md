# python自动发送邮件   

在说python发送邮件之前，需要了解一下简单的邮件发送知识，邮件发送一般通过SMTP协议，你可以把它看作类似于TCP协议一样，我们通过这个协议，然后按照这个协议的格式给数据，就能发送邮件了。  

## 利用smtplib发送邮件   

像把大象装到冰箱里一样，利用python发送邮件也需要个两三步。

###  第一步  

```
import smtplib
from email.mime.text import MIMEText
# 第三方 SMTP 服务
mail_host = "smtp-mail.outlook.com"  # 设置服务器 163的为smtp.163.com，可以去对应邮箱官网去查  

mail_user = "***"  # 用户名
mail_pass = "***"  # 口令/密码或者授权码

sender = '***' # 发信者
receivers = ['***']  #  收信者，可以多个收件人
```
首先需要导入相关的包，然后设置smtp服务器，一般在邮箱官网比如163/outlook的官网就可以找到，用户名一般就是邮箱名，而口令则一般是授权码，你可以去官网或者网上搜一下对应邮箱第三方应用授权怎么弄的，一般设置后会给一个授权码，你就可以拿这个授权码使用第三方应用登陆了。（比如用QQ邮箱客户端登陆163账号）   

[outlook 如何新建授权码（应用密码）](https://support.office.com/zh-cn/article/%E5%B0%86%E4%BD%A0%E7%9A%84-outlook-com-%E5%B8%90%E6%88%B7%E6%B7%BB%E5%8A%A0%E5%88%B0%E5%85%B6%E4%BB%96%E9%82%AE%E4%BB%B6%E5%BA%94%E7%94%A8%E6%88%96%E5%AE%B6%E5%BA%AD%E8%AE%BE%E5%A4%87-73f3b178-0009-41ae-aab1-87b80fa94970)    

163的授权码在设置里面可以找到  

![163授权码](http://ww1.sinaimg.cn/large/005Dd0fOgy1ga7t8e30kmj305y061gm9.jpg)

## 第二步  

```
message = MIMEText(msg, 'plain','utf-8')  # 内容, 格式, 编码
message['From'] = "{}".format(sender)
message['To'] = ",".join(receivers)
message['Subject'] = title
```
这一步主要是写正文，标题。msg对应的就是正文，而From和To对应的是收件人和发件人，title对应标题。把他们封装到message。  

## 第三步  

```  
smtpObj = smtplib.SMTP()
smtpObj.connect(mail_host, 587)  # 587 为 SMTP 端口号
print("connect")   

# smtpObj.ehlo()  # 向邮箱发送SMTP 'ehlo' 命令 针对outlook需要
# smtpObj.starttls()

smtpObj.login(mail_user, mail_pass)
print("login in")
# print(message.as_string())
smtpObj.sendmail(sender, receivers, message.as_string())
print("邮件发送成功")
```

这一步开始真正的连接服务器，并登陆以及发送邮件。这一步也是出错最多的时候。  

这里如果报错需要注意的有几点：  

* smtplib.SMTP()可以换成smtplib.SMTP_SSL()
* port端口号可以尝试 587 465 25  
* smtplib.SMTPNotSupportedError: SMTP AUTH extension not supported by server则检查 smtpObj.ehlo()、smtpObj.starttls()是否被注释掉（163的邮箱注释掉能用，outlook的好像不能用）  
*  554 DT:SPM smtp12 163邮箱可能会有这个错误，则尝试在收件人里面加入发件人    


后话，可能把这个整好了，发现也没什么玩的，因为没有内容可以发，我做这个主要是之前写过一篇用用python自动发微信每日一句，但是后来微信的web接口被封了，所以无法用python登陆了，所以我就将那个脚本修改成了用邮箱发。你也可以爬取一些新闻或者天气以及壁纸发给邮件，做一个信息聚合入口，也挺好玩的。  

之前我用python做的python获取天气预报并微信发送  

https://blog.csdn.net/qq_28888837/article/details/81000857




