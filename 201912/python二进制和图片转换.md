# python二进制和图片转换  

之前做项目，发现很多图片都是存储成二进制的形式，比如存储为`.bin .dat`等文件，如果你直接用记事本打开文件，会发现里面内容如下：  

```
0005 0000 0000 0000 0000 0000 0000 0000
0000 0000 0000 0000 0000 0000 0000 0000
0000 0000 0000 0000 0000 0000 0000 0000
0000 0001 0000 0000 0000 0000 0000 0000
```  
这就说明是把图片转为了二进制字节流保存了起来，你需要通过代码将其转换成图片，下面我就给出转换的方法。  


## 图片转二进制流   

图片如果使用opencv读取出来，本质就是一个numpy，所以直接利用numpy的tofile即可将其保存成二进制流。  

```python

def save2bin_1():
    a = cv.imread("test.png")
    a.tofile("test.dat")

```  

当然你也可以直接以`wb`的方式打开一个文件，然后把数据写进去即可。  

```python  
def save2bin_2():
    a = cv.imread("test.png")
    with open("test1.dat","wb") as f:
        f.write(a)
```  

二者本质是一样的，所以`.dat`里面的数据也是一样的。  

这里有一点需要提的是，我们将数据写入dat里面后，其实不是一个numpy数组了，而变成了一个字节流，所以我们以第二种方式保存的话，我们还能够在字节流后面继续加入数据，比如:  
```python  
def save2bin_2():
    a = cv.imread("test.png")
    with open("test1.dat","wb") as f:
        f.write(a)
        f.write(np.array([1],np.uint8))
```  

一定要注意是uint8，因为这样以后解析的话，可以都按照每个数据都是一个字节。上面的代码其实相当于保存了一个图片和一个label。  

当然如果也可以通过先把np展平，然后再增加一个1，这样保存一张图片和一个label：  

```python 

def save2bin_2():
    a = cv.imread("test.png")
    a = a.flatten()
    a = np.append(a,1).astype(np.uint8)
    with open("test1.dat","wb") as f:
        f.write(a)

```

## 读取二进制流转为图片  

numpy有tofile的方法，同样也有fromfile的方法，所有可以直接使用如下代码读：  

```python  
def load_1():
    data = np.fromfile("test.dat",np.uint8).reshape(332,297,3)
    cv.imshow("a",data)
    cv.waitKey(0)

```  

这里有两个小细节要注意，第一个是数据类型一定要是`np.uint8`,这样才能按照一个数据一个字节解析，第二个是一定要reshape，因为之前说过我们保存之后其实变成字节流了，你其实可以看作是一个字符串/list类似的东西，你可以打印一下看看`print(np.fromfile("test.dat",np.uint8).shape)`，会发现其是一个一维的数据，相当于我们保存的时候，tofile把数据自动展平了，所以我们加载的时候要reshape。  

除了直接用tofile方法，你可以用`rb`的方式打开这个文件，然后读取即可。  

```python  
def load_2():
    f = open("test.dat","rb")
    d = f.read()
    tmp_a = []
    for i in d:
        tmp_a.append(i)
    tmp_a = np.array(tmp_a,dtype=np.uint8)
    cv.imshow("a",tmp_a.reshape(332,297,3))
    cv.waitKey(0)

```  
这个代码就更能体现字节流的概念了，通过`d = f.read()`，我们将数据存到了d中，我们可以直接将d输出，你会发现是这种格式的数据`b'\xff\xff\xff\xff\xff...`，前面的b就代表了是字节，你可以输出d的类型，是`<class 'bytes'>`，我们如果想要取字节流里面的一个个数据，则需要遍历一遍，把每个字节都存起来存到tmp_a里面，这样就自动转成了int，所以tmp_a里面是这样的数据`[255, 255, 255,...]`就是一个普通的list，之后我们将其转成numpy并reshape即可。  









