##python操作excel-xlrd、xlwt
### 工具介绍
python操作excel需要用到两个模块一个是xlrd用于读excel，另外一个是xlwt用于写excel，两个模块安装可以使用pip install直接进行安装。
### xlrd进行读操作

#### 1. 导入模块  

	import xlrd as xr
	import xlwt as xw

#### 2. 打开一个excel,并打开一个sheet（工作簿）
	data = xr.open_workbook(READ_NAME)
	table = data.sheets()[sheet_num]
其中READ_NAME是excel的名字，你可以在之前写READ_NAME="test.xlsx"，sheet_num是第几个工作簿，从0开始计数，你也可以直接用工作簿的名字进行索引工作簿。到这里，你就拿到了一个table，这个table里面就是一张表的信息。

#### 3. 遍历一张表  
	i=0
	while i < table.nrows:  
	        zero=table.row_values(i)[0]
	        one=table.row_values(i)[1] 

其中table.nrows代表共有多少行，table.row_values(i)[0]代表第i行第0列的值。通过上面的代码你就可以进行表的遍历，并且自己通过条件判断，进行表处理。

### xlwt进行写操作

#### 1. 导入模块  

	import xlrd as xr
	import xlwt as xw

#### 2. 创建一个表，并添加一个工作簿
	data = xw.Workbook()
	table=dataw.add_sheet(sheet_name,cell_overwrite_ok=True)
其中sheet_name是你创建工作簿的名字，你可以在之前书写 sheet_name ="sheet1"

#### 3. 向工作簿写入数据

	table.write(0,0,"hello")

上面表示在第0行0列写入hello（其实就是excel中1，A处的数据）

#### 4. 表格式修改

    style = xw.XFStyle()  # 初始化样式
    alignment = xw.Alignment() 
    alignment.horz = xw.Alignment.HORZ_CENTER # 垂直对齐
    alignment.vert = xw.Alignment.VERT_CENTER # 水平对齐
    alignment.wrap = xw.Alignment.WRAP_AT_RIGHT # 自动换行
    style.alignment = alignment
	table.write(0,0,"hello",style)
上面代码先写了一个样式style，然后你再写数据的时候同时加入参数style，这样你的数据就有了格式。

	       