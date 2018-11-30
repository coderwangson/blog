在分析springmvc执行过程之前，先对传统servlet的执行过程进行一个简短介绍，因为二者的职能是一样的，都是控制层的东西。
传统servlet的执行过程分为如下几步：
1、浏览器向服务器发送请求http://localhost:8080/webproject/hello
2、服务器接受到请求，并从地址中得到项目名称webproject
3、然后再从地址中找到名称hello，并与webproject下的web.xml文件进行匹配
4、在web.xml中找到一个 `<url-pattern>`hello`</url-pattern>`的标签，并且通过他找到servlet-name进而找到`<servlet-class>`
5、再拿到servlet-class之后，这个服务器便知道了这个servlet的全类名，所以便可以通过反射技术创建这个类的对象，并且调用doGet/doPost方法
6、方法执行完毕，结果打回到浏览器。结束。

经过上面的介绍，大概了解了传统servlet的执行过程，而springmvc（或者说structs这类框架）所做的就是简化了servlet的一些步骤，因为servlet的开发中一个业务可能就代表一个servlet处理，但是一个servlet中只有一个doget，所以你可以进行两种处理：
1、要么你需要写非常多的servlet。
2、要么你需要在请求url后面加一个叫做method=“xx“的属性，这样你可以在servlet的doget中通过许多if-elseif进行判断并做相应响应。
这两种方法都无疑都显得麻烦，所以springmvc就出现了，springmvc和spring是一家产品，所以使用起来非常好用，和spring无缝对接。
spring所做的就是把所有的请求都拦截下来，然后交给一个叫做DispatcherServlet的东西去处理，这样的话，你就不需要写servlet了，你只需要写一个简单的类就行了，然后写好配置文件，剩下的交给springmvc就行了。

他的处理过程如下（**因为我是初学，所有有些部分是推测的，欢迎指正批评**）
1、同样浏览器发请求到服务器http://localhost:8080/webproject/hello
2、服务器接受到请求，并从地址中得到项目名称webproject
3、然后再从地址中找到名称hello，并与webproject下的web.xml文件进行匹配
4、在web.xml中找到一个 `<url-pattern>hello</url-pattern>`的标签，并且通过他找到servlet-name进而找到 `<servlet-class>`
在这里就与传统的servlet有些不同了，因为传统的servlet的url-pattern一般就是外部访问的映射名，而一般在springmvc中会写 成/ 或者 /*或者  *.action等，如果servlet学的比较扎实的话，应该能看的出来 /是对什么拦截的 /* 是对什么拦截的，/ 就是拦截所有 （具体参考springmvc中介绍）...所以当这个/hello的请求到来后就匹配到了这个url，然后找到了org.springframework.web.servlet.DispatcherServlet这个servlet，所以便交给其去处理，他便会去创建（或者叫做找）servlet。
5、因为springmvc和spring是一家，所以springmvc的对象也可以类似spring那样交给一个容器管理，这样DispatcherServlet便会在这个容器中通过相应的技术找到这个/hello对应的对象（因为有@Controller 、@RequestMapping("/hello")这两个注解，所以DispatcherServlet便可以按照规则找到相应/hello对应方法）
6、找到/hello对应的方法后，便可以调用这个方法（同样可以通过反射）并且响应了。

附录：关于寻找相应对象补充
因为springmvc和spring是一家产品，所以二者很多东西都是一致的，srpingmvc中对象创建也交给一个容器管理（就是IOC技术），你可以通过web.xml

```
   <init-param>
   <param-name>contextConfigLocation</param-name>
   <param-value>classpath:applicationContext.xml</param-value>
   </init-param>
```
这一段代码指出相关配置文件的位置，而配置文件applicationContext.xml中可以通过

```
	<context:component-scan base-package="cn.controller" />
```
这段代码说明其可以扫描那个包，并对其中有注解的类（@Controller或者其他注解）进行实例化。这样springmvc的容器中便有了一些对象以及，所以再根据url中地址名找到@RequestMapping对应方法名，便可以找到相应方法并调用。
