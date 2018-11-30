## matplotlib与seaborn画图  

### matplotlib中pyplot画图  

#### 1.导入pyplot库  

通过import matplotlib.pyplot as plt指令导入pyplot库并且命名为plt，之后就可以在程序中使用该库了。  

#### 2. 画一个sin函数的图像(折线图)  

如果想画一个图像，首先准备的就是数据，比如我想画一个函数图像，那么要有多个x，y，这样plt才能帮你画出来，准备了数据之后穿个plt，最后再使用plt.show()就可以显示出来了。  

	# 生成x数据 从0-10平均分成500份
	x = np.linspace(0,10,500)
	# np.sin(x)就是每个x对应的y值
	plt.plot(x,np.sin(x))
	plt.show()   

plt.plot()就是画了一条折线图，第一个参数接受的是x值，第二个接收的是对应的y的值。折线图可以看出变化趋势。

#### 3.柱状图  

柱状图类似于折线图，只是换了一种表现方式。在代码里面体现如下。  

	x = [0,1,2,3,4,5]
	plt.bar(x,[1,4,5,6,3,2])
	plt.show()  

其中x对应x轴上数据而第二个参数代表每个柱的高度。柱图可以比较不同地方的大小。  

#### 4.散点图  

散点图也只是一种表现方式，同样给出x与y，然后程序就生成一系列散点。  

	x = [0,1,2,3,4,5]
	plt.scatter(x,[1,4,5,6,3,2])
	plt.show()  

这个就会把柱状图的信息转化成散点表示。散点图可以看出密集程度。  

#### 5. 盒图  

盒图的用途可以统计是否一批数据中有些坏点，所以打印出盒图很容易看出来数据存在的坏点。  

	x = [1,2,3,4,5,6,7,8,9,20]
	plt.boxplot(x)
	plt.show()  

这个只需要准备一批数据，而不需要有对应y，上面的代码会打印出一个盒图，你会发现20这个点是一个坏点。如果你想打印多个盒图，只需要把x变成多维就行了。  

#### 6. 子图  

有时候你可以能需要在一张大图上面画好多小图，这时候就用到子图了。   

	figure = plt.figure()
	ax1 = figure.add_subplot(2,2,1)
	ax2 = figure.add_subplot(2,2,4)
	plt.show()  

第一句话代表拿到一个大图，第二三句话代表拿到两个小图，2,2代表把这个大图怎么划分，这里是划分成2行2列了，1,4代表ax1，ax2在2行2列中的位置。  

之后ax1，ax2就可以当做plt使用了，可以画折线图，柱图等。

#### 7. plt的一些方法  

上面是讲的一些画图的方法，比如plot、bar、scatter等，这里说明一些plt的一些公共方法。	
	plt.xlabel("x轴")  

这个相当于给x轴打了个标签，同样还有ylabel。  

	plt.rcParams['font.sans-serif'] =['FangSong']
	plt.rcParams['axes.unicode_minus'] =False  

上面两句话可以解决在画图的时候出现中文问题。  

	plt.xticks(rotation=45)  

就是把x轴上面的每个刻度上面的字进行旋转45度。  

其实plt里面有很多方法，你可以自己发掘，就不一一列举。  

### seaborn画图  

其实seaborn和matplotllib是类似的，只是对于matplotlib的进一步封装，使得matplotlib更加容易上手。  

#### 1. 导入seaborn库  

使用import seaborn as sns把seaborn库导入，并且取一个别名叫做sns。因为seaborn只是对plt的封装，所以还需要引入plt库，引入方式同上。  

#### 2. 小提琴图  

	sns.violinplot([1,1,1,3,3,7])
	plt.show()   

其实小提琴图和盒图非常类似，只是看起来小提琴图更加形象，通过小提琴图你能分析出来哪一块数据分布比较多。  

大多数情况下可以配合pandas读出来的DataFrame格式的数据进行显示。  

	tips = sns.load_dataset("tips")
	sns.violinplot(x="day",y="tip",hue = "time",data=tips)
	plt.show()  

第一个tips就是从seaborn内置的一个数据集加载出来的，里面包含一批数据，并且列名中有day，tip，time，所以这个时候你就可以直接在violinplot中传入data，然后x和y以及hue代表列名，hue相当于一张图里面两个y。这样你就可以更加方便的通过pandas数据格式画图。  

#### 3. 颜色面板  

你在seaborn中可以使用更加漂亮的颜色去表示图案。  

	data = np.random.normal(size=(20,8))
	sns.boxplot(data = data,palette=sns.color_palette("hls",8))
	plt.show()  

上面是画了一个盒图，其他东西都是类似的，只是最后加了一句palette=sns.color_palette("hls",8)，这就相当于选择了一个"hls"画板，并且取出了八种颜色。


#### 4. seaborn中各种图简明介绍  

sns.factorplot(x="day",y="total_bill",hue="smoker",data = tips,kind="bar")  

这个是多面板，你可以改变kind来指定你想要的图。  

sns.pointplot(x="class",y="survived",hue="sex",data=titanic)  

类似于折线图。  

sns.regplot(x="total_bill",y="tip",data=tips)  

线性回归图，可以对数据进行回归，找出一条拟合线。  

heatmap = sns.heatmap(data,annot=True,fmt ="d",cmap="YlGnBu")  

热力图，其中annot代表热力图上是否写字，fmt是写字的格式，camp代表颜色样式。  

	tips = sns.load_dataset("tips")
	g = sns.FacetGrid(tips,col="sex",hue="smoker")
	g.map(plt.scatter,"tip","total_bill",alpha=.7)
	g.add_legend()
	plt.show()   

这个可以把有两个影响因素的分开在两个子图中，你可以自行运行看下结果。

