# Floccus – 跨平台浏览器「书签同步」插件

标签： `windows`

---

一直想要找一个能够做到跨平台的浏览器，之前使用QQ浏览器，虽然能做到win和安卓，但是浏览器本身毛病太多，后来转到chrome，但是chrome对于国内用户并不友好。除了chrome，也推荐大家关注一下微软的Edge chrome，估计以后也能做好，可以转用这个。  

除了借助浏览器本身，其实也可以通过第三方工具，这里推荐Floccus，它的工作步骤是有一个浏览器插件，可以通过这个插件自动把本地的书签处理上传到一个云上（可以自定义，支持webDav的云盘都可以），所以可以做到跨平台，并且也不用担心Floccus以后不更新了，因为它只是个工具，你真正的书签的内容是存储在云盘上的。  

目前 Floccus 可以使用以下同步方法：  

WebDAV ：目前支持 「坚果云、nextcloud / owncloud，box」 等 WebDAV 服务器
Nextcloud 书签 ：利用 Nextcloud 自带的书签应用程序进行同步，可通过 Web 访问。(需要nextcloud v12 以上版本)。
本地文件同步 ：利用 LoFloccus 4 软件将书签同步到本地, 再利用 Dropbox，Syncthing，rsync 等进行跨设备同步。
注意： 浏览器中内置的**书签同步服务**可能会导致同步出现问题, 建议关闭。  

![2019-11-18_111158.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1g920spy3fzj30ir0bmq43.jpg)

注意： nextcloud 中的 bookmarks_fulltextsearch 会导致其中的书签应用程序出现问题, 请不要在 nextcloud 中安装 bookmarks_fulltextsearch。  

这里以坚果云的WebDAV做演示，因为坚果云是国内产品，所以速度比较稳定。  
## 安装Floccus插件  

直接在chrome扩展商店安装即可，如果访问失败可以去 https://github.com/marcelklehr/floccus/releases/  自行下载，使用开发模式安装.crx文件。  

## 准备配置Floccus  

点击添加账户  

![2019-11-18_111716.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1g920vzoq9uj30cf02y74f.jpg)  

选择 `WebDAV共享中的XBEL文件`,你会发现需要配置一堆东西，但是目前这些东西我们还没有，比如`WebDAV URL 用户名`等，这些是需要用坚果云获得的。  
## 坚果云  

坚果云其实就是个云盘，只不过里面有WebDav功能，你可以把这个功能看作一个类似于FTP的功能，你把你的数据上传到坚果云上，之后你可以不用登陆坚果云，而直接通过WebDAV 访问。  

打开坚果云，然后添加一个文件夹，在文件夹下面建立一个bookmarks.xbel 文件，里面内容可以先写个头：  

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE xbel PUBLIC "+//IDN python.org//DTD XML Bookmark Exchange Language 1.0//EN//XML" "http://pyxml.sourceforge.net/topics/dtds/xbel.dtd">
<xbel version="1.0">
</xbel> 
``` 

这个文件就是将来Floccus把书签进行处理，然后写到这个文件里面。  

现在，书签的服务器已经建好，我们在坚果云找到webDav服务：  
通过账户信息 -> 安全选项->第三方应用管理，创建一个WebDAV应用，获取密码 

## 填写Floccus  

现在我们Floccus里面的东西就都有了，WebDAV URL是坚果云的，用户名就是你坚果云的用户名，密码就是刚才你申请webDav的那个密码，书签路径就是坚果云里面那个xbel文件路径。本地文件夹就是你本地书签地址，一般选根目录即可：  

![2019-11-18_113609.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1g921fmp6zjj30bg05vgm9.jpg)   

保存之后，就可以同步了，同步之后，你发现你坚果云上的那个xbel文件里面多了一些东西，多的就是你的书签，他们按照指定格式保存到了xbel文件里面。  

## 安卓端  

需要安装能够安装插件的浏览器，比如kiwi brower/via等，然后同样安装Floccus，配置服务。这里需要注意的是，Kiwi同步目录选择`/移动设备书签`,否则`bookmarks.xbel`会多个移动书签,导致移动端和电脑端分隔开同步;如出现了这种情况,下载`bookmarks.xbel`,用Notepad2打开(因为带代码伸缩)把移动端的书签整体删除即可,一看就明白的,很简单的。 







