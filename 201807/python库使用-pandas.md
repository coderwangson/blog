## python库使用-pandas  

### 一 .打开文件  

通常使用pandas打开一个csv文件，你可以使用read_csv来读取一个csv文件，他的返回值是一个DataFrame类型的数据。假如csv文件格式如下:  
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019032121424573.png)

其中第一行不是数据，而是列的名字，然后其他的行都是有效数据。

### 二 .DaTaFrame的使用  

其实DataFrame你也可以看成是一个大矩阵，里面是很多的数据，只是这个数据没一列都有的名字，你可以使用列名操作矩阵。  

data = pandas.read_csv("data.csv")  

这个data就是一个DataFtame类型的数据，你可以使用data.columns获取列的名字，也就是第一行数据中的内容，用shape整个数据矩阵的大小是多少行与多少列。  

data.loc[1]则可以定位每一行的内容，[]里面可以进行切片比如loc[1:3]就是1-3行的数据。  

如果你想看某一个列的数据则可以data['id'],[]中代表列的名字。  

### 三 .DataFrame常用函数  

#### mean()  
这个函数可以求平均值，比如data['num'].mean()就代表对所有数据的数量进行求均值。  

#### apply()函数  

这个函数可以传入一个函数，这样的话就可以对一个DataFrame按照传入的函数进行处理，比如上面的数据如果你想把某一列的值的格式批量修改，那么你只需要定义一个函数可以修改某个值，然后传入即可。  

比如希望吧num都扩大10倍。  

	def big(row):
		return row['num']*10
	
	data['num']=data.apply(big,axis=1)  

其中big函数中的row其实就是data中的每一行的数据，然后row['num']就是这一行的num数据，返回了一个*10的结果，就会被data['num']的此行接收。这个函数可以用于把连续的值离散化，或者批量改变某个列的数据格式。

#### pivot_table()  

数据透视图，可以用于统计比如：  

	data.pivot_table(index="name",values="num",aggfunc=numpy.sum)  

这个就可以透视出来各种名字不同的物品的各自的总和，其中index就是用来分类的数据，values就是用来统计的数据列，而aggfunc就是说明统计的方式。可以是sum，mean等。最终生成的结果可以使用result.index和reslut.values分别获得。  

### 四. Series  

其实DataFrame中的行和列都是由Series组成的，可以把Series看做是DataFrame的一个小部件。