# Selenium使用浏览器自动登录校园网

标签： `python` `爬虫`

---

校园网需要登录验证，所以采用Selenium操作浏览器直接登录。  

## 打开浏览器  

```  

from selenium import webdriver
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities
import time

# 前台开启浏览器模式
def openChrome():
    # 加启动配置
    option = webdriver.ChromeOptions()
    option.add_argument('disable-infobars')
    # 打开chrome浏览器
    driver = webdriver.Chrome(chrome_options=option)
    return driver
    
```  

代码就是上面的一波操作，调用这个函数便能开启浏览器，但是在这个之前需要几步配置。首先是要下载`chromedriver `，可以在[这个链接][1]里面下载，但是要注意的是版本号要对应。下载完成后，有些说直接把这个驱动放到你的chrome浏览器的根目录下，并且修改环境变量，但是并没用，后来把驱动复制一份到当前py文件下，问题解决。可以成功打开浏览器了。  

## 使用浏览器自动打开网址并且输入表单  

```python
def operationAuth(driver):
    url = "http://***/a70.htm"
    driver.get(url)
    # 找到输入框并输入查询内容
    elem = driver.find_element_by_xpath("//*[@id='edit_body']/div[2]/div[3]/form/input[1]")
    elem.send_keys("***")
    elem = driver.find_element_by_xpath("//*[@id='edit_body']/div[2]/div[3]/form/input[2]")
    elem.send_keys("***")
    # 提交表单
    driver.find_element_by_xpath("//*[@id='edit_body']/div[2]/div[3]/form/input[3]").click()


``` 

参数driver就是第一步拿到的浏览器，其中url是你需要打开的网址，driver.get(url)，下面`find_element_by_xpath`拿到元素，原来我使用的是`find_element_by_name`，但是有时候可能会重名，所以就会报错，所以还是使用id或者xpath，这样能唯一确定一个位置，对于xpath来说，在浏览器的检查元素中可以直接拿到。`send_keys`就是填充表单，最后找到提交按钮，click即可。  

这种方法相对于使用reques这种来说比较简单，因为直接操作了浏览器，并且也不会被反爬虫，但是缺点也是明显的，需要打开一个浏览器，并且不如request简洁，request其实就是左关键的，发送一个关键的表单，拿到需要的内容，而这个方式请求了很多没用的，也发送了很多没用的。不过优点就是可以不用有网络的知识，可以直接使用，不用去分析请求头等。


  [1]: https://chromedriver.storage.googleapis.com/index.html