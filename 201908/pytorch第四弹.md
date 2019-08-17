# pytorch第四弹

标签： `pytorch` `深度学习`

---

上面讲到深度学习的五要素，这次则讲深度学习的第一个要素-输入，只有有了输入，深度学习才能有东西处理，所以输入是第一步。  

## 随机生成数据  

适用场景：当你拿到一个github上的代码，但是没有数据或者数据很大要下载很久，而你想调试一下网络结构，这个时候就能先随机生成一个batch的数据，然后输入到网络里面进行测试：  

```
data = torch.randn(16,3,28,28)
y = model(data)
```  

其中` torch.randn(16,3,28,28)`就随机生生成了一个批次为16，每个大小为28*28，通道为3的数据集，model是一个模型（后面会讲），把数据输入模型就可以了。  

## DataSet  

pytorch是一个面向对象的工具，所以里面的很多实现也是按照面向对象的思想来进行的。我们想要实现一个自己的数据集，并且加载，则只需要继承pytorch的`torch.utils.data.DataSet`即可，并且把里面的一些方法给按照自己想要的方式重写即可。一般里面需要重写的方法有`__init__` `__getitem__`  `__len__`，其中`__init__`是构造方法，可以把一些全局性的信息传入，比如我们数据所在的位置等。`__getitem__`相当于从数据集里面取内容，你可以把数据集看作是一个很大的列表，所以`__getitem__`就相当于从列表里取某个数据，并且还可以做一些数据处理的操作。`__len__`则代表数据集的长度，只需要返回一个数值即可。下面以一个例子来说明如何写一个自己的数据加载。  

加入我们数据都在一个img/文件夹下面存储，类别可以通过文件名解析出来比如`1_fake.jpg 2_fake.jpg 3_real.jpg 4_real.jpg`，通过下划线后面的`fake 或real`进行判断类别。 

```python  
from torch.utils import data
class Data(data.Dataset):  # 自定义数据集，继承Dataset，实现 __getitem__和__len__
    def __init__(self, root, transforms=None):  # 做准备
        self.imgs = glob("%s/*.jpg" % root)
        self.transforms = transforms

    def __getitem__(self, item):
        img_path = self.imgs[item]
        label = 0 if img_path.split('/')[-1].startswith('fake') else 1
        pil_img = Image.open(img_path)
        if self.transforms:
            data = self.transforms(pil_img)
        else:
            array = np.asanyarray(pil_img)
            data = torch.from_numpy(array)
        return data, label

    def __len__(self):
        return len(self.imgs)
        
```  

通过上面可以看出，在init里面接受的参数一般是数据所在的目录以及一个数据预处理(transforms 后面会讲)，并且在init里面把所有的数据的文件名进行解析保存到一个列表里面（self.imgs），所以可以把init函数看作是一个准备操作，把所有的内容都准备好，比如数据的文件名，数据的标签（这里由于标签可以从文件名得到，所以init里面可以不做标签处理），并且把这些内容存作为对象属性。  

getitem里面，只接受一个参数item，本质是一个整数，代表着索引，getitem函数在python中也是一个内置函数，如果实现了这个函数，则对象可以通过下标进行索引，在getitem里面则根据下标以及文件名列表进行对应，得到当前需要的文件的文件名，这里标签则通过文件名解析得到，之后通过读去图片获得一个data的矩阵，再使用transform进行预处理即可。所以每次我们从Data里面拿东西得到的是一个矩阵和一个标签。  

len函数就比较简单，只需要把数据集的长度返回即可（也就是所有文件名列表的长度）  

Data的使用：  

```
dataSet = Data("/imgs",transform)
img,label = dataSet[0]
print(label)
print(dataSet.imgs)
for img,lable in dataSet:
    print(lable,img.size())
```  

因为Data是一个类，则直接创建一个对象即可，构造函数接受的是数据的地址,imgs下面有多张图片，然后取一个数据，则可以直接用`[0]`取得，类似于列表，非常方便好懂。  

## DataLoader  

DataLoader并不是制造数据集的，它只是为了加快数据集的读取，上面我们构造了一个数据集，可以每次取一个数，但是再深度学习中，每次是取一个批次，并且取完之后就开始训练，并且训练时间长，所以如果我们都是按照顺序单进程的进行，则效率低下，所以DataLoader就出现了，DataLoade是再DataSet上面进一步封装，可以给DataLoade几个参数，比如批次大小，是否打乱等，然后DataLoader就会按照多进程的方式读取数据，可以和运算的并行，这样大大加快了训练的速度。  

```python
from torch.utils.data import DataLoader
dataset = Data("/imgs",transform)
# dataset就是集成了data.DataSet的类对象，sampler样本取样 num_works 进程数，collate_fn 如何拼接多个样本一般默认
# pin_memory 是否存在这个内存区域，会快一点，drop_last 可能dataset中个数不是batc的整数倍，drop会把最后一个丢掉
dataloader = DataLoader(dataset,batch_size=2,shuffle=True,num_workers=2,drop_last=False)
dataiter = iter(dataloader) # 一个个批次
imgs,labels = next(dataiter) # 拿到一批次的数据
print("*"*8)
print(imgs.shape,labels)

for data,label in dataloader: # 一个epoch
    print(label)
```  

从上面代码可以看出，DataLoader直接对Data封装，之后可以把dataloade看作一个生成器，可以通过for进行遍历，也可以通过迭代器迭代。  

## 数据预处理 Transform  

这个在DataSet的时候就用到了，只是没细讲，这个只是一个类，只需要知道它的参数即可，学会使用就行了，比如下面代码：  
```
from torchvision import transforms
transform = transforms.Compose( # 多个操作一起
    [
     transforms.Lambda(lambda img:img.rotate(90)),# 通过Lambda进行自定义操作,但一定注意img要支持这种操作，比如rotate
        transforms.Resize(224),# 放缩图片
        transforms.CenterCrop(224), # 中心裁剪
        transforms.ToTensor(), # 转为Tensor，并且归一化为[0,1]
        transforms.Normalize(mean=[.5,.5,.5],std=[.5,.5,.5]), # 标准化到[-1,1]
       

    ]
)
```  

上面就是定义了多个预处理操作，对图片进行缩放，中心裁剪，注意下面的**ToTensor**,在ToTensor之前都是对图片的操作，也就是图片有的，比如缩放，裁剪这种，而ToTensor下面的是对Tensor的操作，比如求均值，标准化等。  

除了这些简单的操作，还有一个Lambda，这个比较强，因为可以实现一些自定义的操作，比如有些操作pytorch不支持，但是你加载的数据是PIL的，而PIL支持，那么就可以调用，比如上面的img.rotate,在pytorch里面没有旋转操作，而DataSet读取数据的时候是用PIL读取的，而PIL读取的Image有rotate的，则通过Lambda就可以直接操做了。这些在Tensorflow操作起来都比较麻烦。