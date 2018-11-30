### jeesite中日期格式问题
在使用jeesite进行快速开发的时候发现日期类型有问题，有时候会出现类型不能比较的错误，在网上搜索发现我自动生成的数据库映射文件中有这样一行代码
```
		<if test="beginOrderTime != null and endOrderTime != null and beginOrderTime != '' and endOrderTime != ''">
				AND a.order_time BETWEEN #{beginOrderTime} AND #{endOrderTime}
			</if>
```
其中罪魁祸首就是endOrderTime != ''，如果在mybatis中使用这句话就会把日期转换成String类型，所以会报错，经测试将！=''去掉就行了

除了这个地方有问题，对于日期来说，jeesite前端在使用那个日期选择插件有时候也会出现问题
```
onclick="WdatePicker({dateFmt:'yyyy-MM-dd HH:mm:ss'
```
这个地方如果有 HH:mm:ss则可能会出现即使你选择了日期，后台在校验的时候也会出现日期为空的提示，解决方法就是把 HH:mm:ss去掉就行了。