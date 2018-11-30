## spring项目在web中测试
如果在普通的web项目中，其测试非常简单，因为所有的东西你都可以自己new，但是到了使用spring技术的web项目中，一切就没有这么简单了，因为在spring中我们把对象都交给了spring容器，如果我们直接部署到服务器运行，那么tomcat服务器会帮我们进行spring的初始化--这个在web.xml文件中你配置的那几个标签都有所体现。但是我们仍然需要在spring的web项目中进行测试，这个时候就需要整合Junit和spring了。

最简单的方法就是写一个测试类，然后在测试类中手动加载spring的xml文件：
	ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:config/application-context.xml")
之后就可以从ac中拿到对象了。

上面虽然可行，但是可能看着不舒服，所以可以使用第二种方法，使用注解

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:config/application-context.xml") 
```
然后在测试方法中就可以直接使用spring容器中的对象了，不过前提你要把他注入到这个类中。

---

上面讲的都是基本，但是我在进行操作的时候出现了很多错误，现在分享一下。  

1. java.lang.NoSuchMethodError: org.slf4j.spi.LocationAwareLogger.log  
  出现这个错误的原因是slf4j这个jar包版本冲突，因为我的项目不是maven项目，所以里面有很多jar包，其中我详细找了一下，最终发现是这三个冲突了logback-classic-0.9.27.jar、slf4j-jdk14-1.6.2.jar以及我javaee6.0中的自带的一个jar包冲突了，所以解决方案就是删除logback-classic-0.9.27.jar，以及把javaee6.0修改成5.0就行了。更改方法参考[javaee6.0修改成5.0](http://？)
2. 上面搞好后，虽然可以加载xml文件了，但是总会出现一个org.springframework.beans.factory.BeanInitializationException: Could not load properties的错误，翻译过来就是加载不到配置文件就是properties文件，最后我的解决方案就是把我所有的配置文件从webroot下面迁移到src下（就是复制一份，src下单用作测试，而webroot下面给服务器用），因为我的配置文件都在一个文件夹内，所以修改起来简单些。
