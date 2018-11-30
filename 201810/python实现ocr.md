##  python实现ocr  

###  前期准备    

在这个阶段主要准备整个小程序的结构，既然要实现ocr，那么输入就是一张图片，而图片这里采用屏幕截图的方式获得，输出是文字，这里采用搜狗的ocr接口，我们把截好的图片传到搜狗ocr接口中，然后把返回的文字作为输出即可。   

由于想做一个小程序，所以要为程序做GUI，这里采用tkinter编制GUI界面。   

### 界面编写  

界面主要就准备一个窗体，里面有菜单，给出OCR功能。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181028152618872.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_27,color_FFFFFF,t_70)   
之后我们点击菜单，则启动一个截图功能，在截图完成后，我们就把截得的图片传入ocr接口并返回文字到主窗体中。  

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181028153451202.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3,size_27,color_FFFFFF,t_70)  

主面板的编写则直接使用tkinter建立菜单等  

		root = Tk() 
		root.title("小新的OCR")
		# 创建一个顶级菜单
	    menubar = Menu(root)
	    # 创建一个下拉菜单“文件”，然后将它添加到顶级菜单中
	    filemenu = Menu(menubar, tearoff=False)
	    filemenu.add_command(label="OCR", command=buttonCaptureClick, accelerator='Ctrl+N')
	    filemenu.add_command(label="帮助",command=helpClick)
	    filemenu.add_command(label="退出", command=root.quit)
	    menubar.add_cascade(label="操作", menu=filemenu)
	    # 显示菜单
	    root.config(menu=menubar)
	    root.bind_all("<Control-d>", lambda event: buttonCaptureClick())
	    #启动消息主循环
	    root.mainloop()  

这样变回产生一个窗体，用户可以和这个窗体进行交互，你可以点击菜单，然后找到其子菜单中的OCR一项，点击它便会调用一个buttonCaptureClick的函数，这个函数就来产生截图，并且保存截图。  

### 截图功能实现  

截图功能我也是参考网上的内容，原理就是先把整个屏幕给捕捉到，然后监听鼠标事件，当鼠标左边按下则作为截图的左顶点，鼠标左键松下则最为截图的右底点，这样我们截图区域就出来了，然后进行保存即可。  

```
#用来显示全屏幕截图并响应二次截图的窗口类
class MyCapture:
    def __init__(self, png):
        #变量X和Y用来记录鼠标左键按下的位置
        self.X = tkinter.IntVar(value=0)
        self.Y = tkinter.IntVar(value=0)
        #屏幕尺寸
        screenWidth = root.winfo_screenwidth()
        screenHeight = root.winfo_screenheight()
        #创建顶级组件容器
        self.top = tkinter.Toplevel(root, width=screenWidth, height=screenHeight)
        #不显示最大化、最小化按钮
        self.top.overrideredirect(True)
        self.canvas = tkinter.Canvas(self.top,bg='white', width=screenWidth, height=screenHeight)
        #显示全屏截图，在全屏截图上进行区域截图
        self.image = tkinter.PhotoImage(file=png)
        self.text =""
        self.canvas.create_image(screenWidth//2, screenHeight//2, image=self.image)
        #鼠标左键按下的位置
        def onLeftButtonDown(event):
            self.X.set(event.x)
            self.Y.set(event.y)
            #开始截图
            self.sel = True
        self.canvas.bind('<Button-1>', onLeftButtonDown)
        #鼠标左键移动，显示选取的区域
        def onLeftButtonMove(event):
            if not self.sel:
                return
            global lastDraw
            try:
                #删除刚画完的图形，要不然鼠标移动的时候是黑乎乎的一片矩形
                self.canvas.delete(lastDraw)
            except Exception as e:
                pass
            lastDraw = self.canvas.create_rectangle(self.X.get(), self.Y.get(), event.x, event.y, outline='black')
        self.canvas.bind('<B1-Motion>', onLeftButtonMove)
        #获取鼠标左键抬起的位置，保存区域截图
        def onLeftButtonUp(event):
            self.sel = False
            try:
                self.canvas.delete(lastDraw)
            except Exception as e:
                pass
            sleep(0.1)
            #考虑鼠标左键从右下方按下而从左上方抬起的截图
            left, right = sorted([self.X.get(), event.x])
            top, bottom = sorted([self.Y.get(), event.y])
            pic = ImageGrab.grab((left+1, top+1, right, bottom))
            fileName ="temp.jpg"
            pic.save(fileName)
            self.text = get_text(fileName)
            #关闭当前窗口
            self.top.destroy()
        self.canvas.bind('<ButtonRelease-1>', onLeftButtonUp)
#让canvas充满窗口，并随窗口自动适应大小
        self.canvas.pack(fill=tkinter.BOTH, expand=tkinter.YES)
 #开始截图
def buttonCaptureClick():
    #最小化主窗口
#     root.state('icon')
    root.withdraw()
    sleep(0.4)
    filename = 'temp.png'
    #grab()方法默认对全屏幕进行截图
    im = ImageGrab.grab()
    im.save(filename)
    im.close()
    #显示全屏幕截图
    w = MyCapture(filename)
    root.wait_window(w.top)
    #截图结束，恢复主窗口，并删除临时的全屏幕截图文件
    root.update()
    root.deiconify()
    text1.config(state = NORMAL)
    text1.delete(0.0,END)
    text1.insert('insert',w.text)
    text1.config(state = DISABLED)
    text1.pack()
    os.remove(filename)
```

### OCR实现  

因为OCR其实是采用了搜狗的接口，所以需要做的工作也不是很多，只需要把我们的图片传入即可。  

```
def get_text(img_path):
    print("")
    img = img_path # 图片路径
    files = {"pic_path": open(img, "rb")}  # files # 类似data数据
    url = "http://pic.sogou.com/pic/upload_pic.jsp"  # post的url
    keywords = requests.post(url, files=files).text  # requests 提交图片
    url = "http://pic.sogou.com/pic/ocr/ocrOnline.jsp?query=" + keywords  # keywords就是图片url此方式为get请求
    ocrResult = requests.get(url).json()  # 直接转换为json格式
    
    contents = ocrResult['result']  # 类似字典 把result的value值取出来 是一个list然后里面很多json就是识别的文字
    text = ""
    for content in contents:  # 遍历所有结果
        text+=(content['content'].strip()+'\n')  # strip去除空格 他返回的结果自带一个换行
    return text
```

### 内容显示  

内容显示是在截图结束后我们把ocr识别的内容存储起来    
	
	self.text = get_text(fileName)  
然后再显示到主窗体上    

	    text1.config(state = NORMAL)
	    text1.delete(0.0,END)
	    text1.insert('insert',w.text)
	    text1.config(state = DISABLED)
	    text1.pack()  

### 总结   

虽然是一个完整的项目，但是其中的很多模块其实都是借用其他人的模块，而我做的只是把他们结合起来做成一个小项目，所以是站在巨人的肩膀上开发。   

参考：
> https://cloud.tencent.com/developer/article/1097904  
> https://morvanzhou.github.io/tutorials/python-basic/tkinter/  
> https://www.52pojie.cn/thread-708177-1-1.html


