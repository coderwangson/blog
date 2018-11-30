### jeesite快速开发
#### 0.把整个平台导入项目（不介绍 [导入平台](https://blog.csdn.net/u013378306/article/details/52577497 "")）
#### 1.建立数据库
建立数据库之前需要看平台的配置文件system.properties，这个文件在JavaResources下面的src/main/resources/，然后找到其关于数据库的配置信息
```
	jdbc.type=mysql
	jdbc.driver=com.mysql.jdbc.Driver
	jdbc.url=jdbc:mysql://localhost:3306/bucea?useUnicode=true&characterEncoding=utf-8
	#jdbc.url=jdbc:mysql://192.168.0.110:3306/bucea?useUnicode=true&characterEncoding=utf-8
	jdbc.username=root
	jdbc.password=root
```

其中jdbc.url后面就是数据库的名字。（数据库的初始化可以参考导入平台中的数据库部分）
然后在这个数据库中可以建立自己的表，注意因为使用的是别人的开发工具，所以可能建表的时候也要按照别人的规则来。


####2.部署项目并运行
把项目导入到ecplise中，并且启动，然后在浏览器中输入地址，便可以访问。

-----
####3.快速开发-业务表配置
点击菜单栏中的快速开发，然后业务表配置。
![](https://i.imgur.com/R6Ktl1H.jpg)

然后点击业务表添加
![](https://i.imgur.com/toTFqUa.jpg)
点击添加后，便会进入一个表配置的界面，在这个界面中你会配置一些信息，这些信息会和你未来生成的Java代码相关。
![](https://i.imgur.com/h8thfOG.jpg)
在这里着重说一下**父表表名** --父表代表你当前表如果有外键的话，你这个外键的引用表的表名。比如当前有两张表：表 a(id,name),表b(id,name,a_id)，其中b表中的a_id是一个外键，引用的是a表中的主键，所以此时在配置b表的时候就需要加入父表表名为a，当前表的外键为a_id。这个在将来生成Java代码的时候会在a表的实体类里面加入一个b表对应的实体类的一个List，而在b表的实体类中会加入一个a表的a_id。其实在这里也能看出来a表对应b表是一对多的关系，而b表对应a表是一对一的关系。

![](https://i.imgur.com/ow5QzMF.jpg)

下面的字段列表就是基本信息了。可空如果没有勾选，将来在插入数据时会有非空校验，同样的插入勾选则在后面插入数据的时候会有该字段，匹配方式则可选等于，like，between这在将来对应生成mybatis的xml中sql语句的写法，而显示表单类型也可选在文本，下拉列表等，你可以自行尝试。而字典类型中一般要选择你系统中的字典，这样如果这个表单类型你选择的下拉列表、按钮则上面的数据会从字典中取到。（字典可以自定义，以后有机会会说）。  然后点击保存就行了。
#### 4.快速开发-生成方案
这个主要是在表配置好之后，开始进行代码生成，在生成代码时候的一些配置。  
点击生成方案添加，然后便可以进行配置
![](https://i.imgur.com/7286SvS.jpg)

注意的就是模板分类可以选择一对多，以及业务表名一定要选择。
还有就是在点击保存并生成代码之前要做的事情就是先设置一下生成代码的路径。  
同样找到system.properties,然后找到 projectPath=F\:\\xxx\\project\\xxx，这里的路径就选择你的项目在的路径就行了。

#### 5.大功告成
下面就是部署运行就行了。可能你运行的时候，这个菜单没有用，这样的话你就修改一下菜单配置。
![](https://i.imgur.com/vVyvdyD.jpg)
