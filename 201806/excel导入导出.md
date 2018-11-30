### Excel导入导出
#### 导出
导出相对比较简单在控制层的关键代码为
```
 String fileName = "成果数据"+DateUtils.getDate("yyyyMMddHHmmss")+".xlsx";
            List<PersonnelTitle> personnelTitleList = personnelTitleService.findList(personnelTitle);
    		new ExportExcel("成果数据", PersonnelTitle.class).setDataList(personnelTitleList).write(response, fileName).dispose();
    		return null;
```
关键之处在于需要在实体类上进行注解
```
	@ExcelField(title="姓名", align=2, sort=10)
	public String getName() {
		return name;
	}
```
这样excel处理程序就知道每个字段对应的excel中的列了。

#### 导入
导入本身比较简单，但是由于我这个项目是基于别人的项目，所以要修改一些地方。
其中核心代码为
```
            //file为前台表单传过来的file
			ImportExcel ei = new ImportExcel(file, 1, 0);
            //这样就把excel转化为了一个list
			List<PersonnelTitle> list = ei.getDataList(PersonnelTitle.class);

```


-----
正常的上面就ok了但是我的导出需要配置几个文件

* system.properties文件  
   需要在upload_spring=后面加上你的上传控制器的url。

* spring-mvc.xml文件  
   <mvc:interceptor>中的<mvc:exclude-mapping增加一个你的控制器url。

* web.xml文件  
  ![](https://i.imgur.com/N3830vt.jpg)
  在param-value中加一个你的控制器url。

