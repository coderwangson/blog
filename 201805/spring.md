## spring入门

### spring是什么以及用途
spring是一个免费开源的框架，可以用在Javaweb中用作对象管理，在我简单的看来，spring可以看做是一个容器。这个容器里面装的东西就是一些对象，通过这样的方式，我们程序员在使用对象的时候就不需要自己创建，而是向spring容器要，这样的话就可以减低程序耦合，提高开发效率。

### spring使用场景
可以在web开发中作为一个对象管理容器，这样我们在开发的时候，把所有的对象的交个spring容器管理，不需要手动的创建。这样代码之间的耦合可以降低。

比如如下场景： 原来你的项目分为三层dao、service、servlet，在service可能需要用到dao层的对象，所以你可能会写这样的代码

```
UserDao udao = new UserDaoImpl();
```

显然这样能解决问题，但是你会发现如果你在修改了Dao层的代码，比如你可能要换了一个实现类用，所以你要修改sercvice层代码

```
UserDao udao = new UserDaoImpl1();
```

这样问题看似得到了解决，但是明显的发现模块间存在耦合，在实际开发中，我们想要的可能是修改了dao层，但是service不动，所以这个时候spring就有了用处。

通过spring，我们可以把对象放在spring容器中，这样我们service层代码就变成了

```
UserDao udao = spring容器.get("对象名");
```

这样的好处是，即使你修改了dao层代码，你所需要做的只是再修改一下spring的配置文件就行了，而不需要对service进行修改，所以这样便达到了解耦的效果。
### sprig的使用方法
首先介绍在普通Java下的使用，非web项目

#### 1.导入相应的jar包，可以去spring官网下载，初学者可以全部导入。

#### 2.写一个类，因为要做对象存储到容器中，所以需要一个类
```
class User{具体代码自己写啦}
```  

#### 3.书写spring的配置文件
从道理上讲这个配置文件的位置，以及这个配置文件的名字都没有任何要求，但是推荐这个配置文件放在src下面，然后名字叫做 applicationContext.xml。
四、下面就是书写这个配置文件了

```


	<beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:aop="http://www.springframework.org/schema/aop"
	    xmlns:tx="http://www.springframework.org/schema/tx"
	    xmlns:context="http://www.springframework.org/schema/context"
	    xsi:schemaLocation="
	   http://www.springframework.org/schema/beans 
	   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	   http://www.springframework.org/schema/aop 
	   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
	   http://www.springframework.org/schema/tx 
	   http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
	   http://www.springframework.org/schema/context      
	   http://www.springframework.org/schema/context/spring-context-3.0.xsd">
	
	  
	</beans>


```


上面就是整个配置文件的体，你可以在beans标签之间写bena，而这个bean的意思就是在spring容器中配置一个对象。

```
  <bean class="spring.User" name="user"> </bean>
```
上面这个bean就是代表配置了一个对象，class属性代表这个对象的类的全类名，而name就相当于做了个标识，将来你从spring容器中取数据时就使用这个名字进行取。

#### 4.测试
上面做完之后，就说明已经有一个对象装在spring容器中了，然后可以写一个测试类，把这个对象取出来。
```
	public static void main(String[] args) {
	    //加载配置文件
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
	    //从spring中取得数据
		User u =(User) ac.getBean("user");
		System.out.println(u);
	}
```
上面会打印出来一个值，就说明对象已经创建成功了。

### 总结
spring就是用来存储对象的，使用步骤就是导入jar包，然后书写xml文件，把你需要的对象配置在里面就行了，最后就是通过ApplicationContext拿到对象。

