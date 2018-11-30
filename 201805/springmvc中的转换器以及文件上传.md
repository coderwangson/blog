转换器：

什么是转换器：
转换器可以把前台传入的数据进行一定的处理，再交给控制器处理。
转换器的作用：
比如前台传入的字符串，你可以使用转换器把字符串的所有空格给去掉，或者前端传入的日期，你可以按照你喜欢的格式进行格式化。
怎么用转换器：
写一个转换器类（就是实现了一个接口的类），然后写配置文件（就是在xml文件中写，主要是利用spring的ioc技术创建这个对象）

下面就是一个转换器的类

//其中Converter就是那个接口，<,>中的两个泛型，第一个是传入时（前台传入）的类型，第二个是转换后的类型（要交个控制层使用）

```
public class Sconvert implements Converter<String, String>{

	@Override
	public String convert(String arg0) {
		return arg0+"00000";
	}

}
```


配置文件如下：


	

```
<mvc:annotation-driven conversion-service="conversionServiceFactoryBean"/>
	<bean id="conversionServiceFactoryBean" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
	<property name="converters">
		<list>
			<bean class="cn.controller.Sconvert" />
			<bean class="cn.controller.Sconvert2" />
		</list>
	</property>
	</bean>
```


配置文件解释如下

![这里写图片描述](https://img-blog.csdn.net/20180502144332557?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI4ODg4ODM3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
总结：我觉得其实转换器就相当于一个变形的过滤器，都是在控制器之前发生的，只是转换器是在交给控制器之前进行了数据的处理，这样更加方便控制器使用相应的数据。

文件上传：
文件上传就是和以前servlet所学的文件上传功能是一样的，只是使用了springmvc之后，他帮你处理了，所以你使用起来就比较方便。
具体步骤：

导入相关jar包，然后写一个带上传功能的jsp，jsp如下

关键部分就是那个enctye，然后还要注意input中的name要后和后台处理函数的那个参数名字要一致

```
<form method="post" action="/springweb/file" enctype="multipart/form-data">
<input name="picFile" type="file"/>
<input type="submit">
</form>
```

后台控制器代码

```
	@RequestMapping("/file")
	//                        这个参数名字要与前台input的name一样
	public ModelAndView file(MultipartFile picFile) throws  Exception{
		
		
		picFile.transferTo(new File("")); //可以把图片转存到一个文件中
	
		
		
	}
```

配置上传解析器，这个还是在xml文件中写，同样是利用了spring的ioc技术，创建了一个对象

```
<!-- 文件上传,id必须设置为multipartResolver -->
<bean id="multipartResolver"
	class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
	<!-- 设置文件上传大小 -->
	<property name="maxUploadSize" value="5000000" />
</bean>
```

总结：就是写一个上传jsp，然后配置一个文件上传解析器。

总总结：
通过上面两个例子可以看出来，springmvc的一些开发步骤。主要就是在xml中配置一些解析器对象（利用的就是ioc技术），然后有些可能需要自己自定义一些相关类，剩下的就交个springmvc去处理就行了。
