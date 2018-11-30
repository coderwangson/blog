## python库的使用-Numpy

### 一. numpy的生成矩阵的使用  

#### numpy.array(object)  

这个方法可以把一个数组转化成一个ndarray类型（可以当做一个矩阵）的数据。  如：  

	vector = numpy.array([[1,2,3],[4,5,6]])  

#### numpy.arange([start,] stop[, step,], dtype=None)  

生成一个指定范围的矩阵，start和stop分别是开始和结束，而step是步长，比如：  

	numpy.arange(10,15,1)  

代表生成一个从10到15 步长为1的一维矩阵。  

#### numpy.zeros(shape, dtype=float, order='C')  

表示生成一个零阵，其中主要是shape代表这个矩阵的形状，传一个元组，比如（3,4）代表一个3行4列的零阵。如:  

	numpy.zeros((3,4),dtype=np.int32)  

代表生成一个3行4列，类型为int的零矩阵。  

#### numpy.random.random(shape)  

表示生成一个矩阵，每个数据的范围是（-1,1），而shape代表着这个矩阵的形状，如:  

	numpy.random.random((2,3))  

表示生成一个2行3列的随机矩阵。  

#### numpy.linspace(...)  

这个代表把一个区间的数据均分成n分，然后生成一个矩阵，如:  

	numpy.linspace(1,10,7)  

就代表把1-10均分成7份然后变成矩阵。  

### 二. ndarray的使用  

对于上面的numpy生成的矩阵在python中用数据类型就是一个ndarray的类型，所以这里对ndarray的方法的使用，就是对上面生成矩阵的方法的使用。  

	a = numpy.random.random(shape) 

#### a.astype(..)  

一个ndarray类型的变量调用astype可以改变整个矩阵的数据类型。如：  

	a.astype(float)  

就把整个矩阵都变成了float类型的了。  

#### a.min() a.sum(axis=)  

其中min明显就是求最小值，sum就是求和的意思，而axis代表是按行求和或者按列求和，axis = 1代表按照行，axis=0 代表按照列。  

#### a.T a.ravel()  

a.T的输出就是a的一个转置矩阵，而a.ravel()就是把一个多维矩阵拉伸成一维的矩阵。

#### 矩阵的运算  

假如有两个ndarray类型的矩阵a,b。那么a-b就是a和b对应位置元素相减(前提二者维度相同)。a*b就是a和b对应元素相乘，a.dot(b)代表把a和b进行矩阵的乘法。  

除了上面的四则运算还有逻辑运算，如a==1就会返回一个True，False矩阵，这个矩阵中如果a的摸个位置为1，则这个TF阵中为True，如a为 [1,2,1,3,4] 则a==1 为[True,False,True,False,False]。  

并且如果把这个TF阵放在a[.]的[]那么就会把所有为T的位置的元素返回出来，比如a[a==1]就会打印出所有a中为1位置的元素。  

### 三. numpy对矩阵的操作  

#### 合并矩阵 numpy.hstack((a,b))  

这个代表把a，b矩阵水平拼接起来，注意参数要是一个元组。而 numpy.vstack((a,b))代表把a，b矩阵垂直拼接。  

#### 拆分矩阵 numpy.hsplit(a,2)  

这个代表把a矩阵拆成两份，如果第二个参数你可以传入一个元组，这样就可以在任意你指定的位置切，比如：  

	np.hsplit(a,(1,3))  

就代表在1和3的位置各切一次。同样还有一个numpy.vsplit(a,2)与hsplit只是方向不同，一个水平一个是垂直。  

#### 扩展矩阵 矩阵排序  

numpy.tile(a,(2,3))代表把a矩阵行扩大2倍，列扩展成三倍。   

numpy.sort(a,axis=0) 代表对矩阵每一列排序。  

### 四. 矩阵的复制

#### a=b  
  
直接a=b那么a和b是一样的，并且a改变了b也会改变，二者id相同  

#### a=b.view()  

这样的话a和b的值相同，并且指向一个矩阵，a中的值变化了会影响b的值。但是二者id不同。  

#### a = b.copy()  

这样的话a只是和b开始时值一样，之后a的值改变不影响b的值，a和b的id不同。

  


 

