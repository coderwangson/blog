# TypeError: an integer is required (got type tuple) RandomResizedCrop

标签（空格分隔）： `pytorch`

---

这个错误，可能是因为版本引起的，但是我在网上没有找到解决方案，自己查看源码发现了问题。  

代码如下：
```
 from PIL import Image

img = Image.open("face-265.bmp").convert('RGB')
transforms.RandomResizedCrop((128,128), scale=(0.9, 1), ratio=(1, 1))(img)
```
RandomResizedCrop这个Transform出错了，原因是输入是一个tuple，但这段代码别人能运行，所以应该版本原因，所以我去`RandomResizedCrop`里面查看了一下，发现：  

```
   def __init__(self, size, scale=(0.08, 1.0), ratio=(3. / 4., 4. / 3.), interpolation=Image.BILINEAR):
        self.size = (size, size)
        self.interpolation = interpolation
        self.scale = scale
        self.ratio = ratio
```  

所以它的输入size应该是一个值，而不是一个tuple，所以代码改为：  

```
 from PIL import Image

img = Image.open("face-265.bmp").convert('RGB')
transforms.RandomResizedCrop(128, scale=(0.9, 1), ratio=(1, 1))(img)
```



