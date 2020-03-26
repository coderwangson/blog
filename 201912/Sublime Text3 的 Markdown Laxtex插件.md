# Sublime Text3 的 Markdown Laxtex插件  

## 准备工作   

* 安装MikTex，可以在windows下使用Latex，我们之后再sublime里面使用的插件其实就是调用这个包。[下载地址](https://link.zhihu.com/?target=http%3A//www.miktex.org/download)

* Sumatra PDF，之后sublime使用它进行pdf预览，[下载地址](https://link.zhihu.com/?target=http%3A//www.sumatrapdfreader.org/download-free-pdf-viewer.html)

## 安装LaxtexTools  

按组合键 Ctrl+Shift+P，然后再输入 install，选择 Package Control: install package。进入库后，搜索所需的包，然后选择安装就好了。我们需要使用的包是 LaTeXTools。  

![](https://pic4.zhimg.com/80/21b3cf36de566d16854292351382e007_hd.jpg)

### 配置LaxtexTools  

![Sublime Text3 的 Markdown Laxtex插件_1.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1ga4d2xo8d8j30fg0do78g.jpg)   

配置"texpath"选项，将MikTex的路径添加进去就可以了，对于sumatra也做同样的，将路径写到里面。  

![Sublime Text3 的 Markdown Laxtex插件_2.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1ga4d6cjcohj30mo07d43h.jpg)  

### 使用  

进行到现在，理论上应该就已经配置好了。以后就可以用 Sublime Text 写 LaTeX 了。写完之后保存（新建的文件一定要先保存，否则 build 是无效的），然后按下快捷键 Ctrl+B，Sublime Text 就会自动调用 LaTeXTools 的 build 系统来进行编译，然后自动打开 SumatraPDF 进行预览。之后每次修改后只要 Ctrl+B 一下，SumatraPDF 里的内容就会自动更新。

### 配置SumatraPDF的反向搜索    

打开软件SumatraPDF，然后设置-->选项，然后最后一个输入框输入你sublime的位置以及`%f:%l`，这样当你用crtl B编译后，跳出一个pdf，你可以点pdf里面的字，然后在sublime里面会展示其对应的位置。   

![](https://upload-images.jianshu.io/upload_images/1957089-daee57120931f89f.gif?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)


![Sublime Text3 的 Markdown Laxtex插件_3.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1ga4dachp2wj30a00bkmyr.jpg)












