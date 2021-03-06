# Yolo v1 解析  

yolo是一个非常优秀的目标检测算法，由于其使用了端到端的方案，并且对图片进行一遍遍历就能够得出结果，所以其速度非常快。虽然yolo v1的精度不是很高，但是其速度非常快，所以在工业界应用很普遍。但yolo的论文读起来非常的难懂，所以这里就结合代码来分析一下yolo v1。   

我们主要通过三方面进行分析： 网络结构，数据集构造以及损失。  

## yolo简单介绍  

yolo首将一张图片看成`S*S`的小格子，如果有一个目标的中心在一个小格子里面，那么这个小格子就对这个目标负责了（responsible），即这个小格子负责产出这个目标的bbox以及cls。

![2020-02-28_171825.jpg](http://ws1.sinaimg.cn/large/005Dd0fOly1gcc8l10mlaj308804mq3n.jpg)  

如上图，我们将图片看做3*3的，可以看出人的中心点在[1,1]这个位置，而狗的中心在[1,0]的位置，所以以后这两个小格子就分别对人和狗负责。既然我们需要这些小格子对目标负责，所以我们网络的输出应该为`S*S*?`，`S*S`很好理解，因为我们将图片看作`S*S`了，所以最终需要`S*S`的矩阵来分别代表这些格子。而后面的？则代表我们一个目标，如果我们想要一个目标，那么我们都需要什么内容呢，首先我们需要知道这个格子里面是否有目标，然后需要目标的bbox，最后是目标的类别（一般用one-hot），既然这样，加入我们使用的是voc数据集，我们最终的输出应该是`S*S*(1+4 +20)`,但我们发现很多代码里面最后的输出是`S*S*30`，那么这个30是怎么来的呢。其实只是将每个小格子的能力增大了，因为在密集的目标图片中，我们可能一个小格子里面会有多个目标，如果是30的话，我们是假设每个小格子里面最多可能两个目标，所以变为了`S*S*(2*(1+4) +20)` ,这个概念可以先放着，因为不好意理解。通过上面的分析，我们大概了解了yolo的流程。下面分析一下网络结构。

## 网络结构  

其实yolo v1的网络结构比较容易理解，就是一个端到端的卷积网络，和之前不同的是，里面没有全连接层，他引入了一个FCN的概念，从上面的介绍可以看出，我们网络的最后一层产出是一个`S*S*?`的特征图，所以在yolo里面可以不使用全连接，直接全部都用卷积即可。FCN里面重要的是一个1*1的卷积核，使用这个卷积核我们就可以在不改变特征图大小，只改变特征图通道，也就是可以自定义最后的？了。所以其网络结构相对容易理解，只是最后变成了FCN而已。  

## 数据集构造  

我们一般使用的目标检测的数据集，里面都是图片以及对用目标的bbox，但通过上面yolo过程的分析，我们需要的是一个`S*S*?`的label，那么我们如何将原来的图片以及bbox构造成一个这样的label呢。首先我们需要构造一个空的大小为`S*S*25`的Tensor为label，之后我们对这张图片的所有bbox进行处理，我们拿到一个bbox之后，我们就能通过这个bbox的位置推出来其所在的小格子（因为我们知道图片的大小以及bbox的位置，所以二者可以计算出来对应的格子）。当得到了其在的小格子，比如[2,3]这个小格子，那么我们就将label[2,3]拿出来，将这个位置的25个值赋值，

`label[2,3,0:4] = bbox,label[2:3:4] = 1, label[2,3,4+cls_index]=1`  

通过上面的赋值我们可以看出，我们把前四个位置赋值为bbox,第4个位置代表是否有目标，而后面的20个则用one-hot代表类别。这样我们就把数据集构造出来了，输入时图片，label是一个`S*S*25`的矩阵。  

上面只是简单的构造的思路，但实际的代码中，还会有很多技巧，因为我们的图片都是很大的，所以首先需要先进行归一化，把bbox的四个坐标归一化到0-1,归一化的过程如下，加入bbox组成是(x,y,w,h)，其中x，y为中心点坐标，wh为目标的宽高，则归一化后为(x/W,y/H,w/W,h/H)，WH为图片的宽高。  

第二个技巧是在实际中，我们不是用的目标的中心点，而是用的目标中心点相对于格子左上角的距离，并相对格子做归一化。比如我们的中心点坐标是(x,y)这个x，y其实是相对图片左上角的，也就是以图片左上角为原点，但是这样的话我们对不同的目标会不公平，比如里图片原点近的，和离图片原点远的对应的值大小有差距，但是二者的重要性实际上是一样的，而将其变换为相对格子的左上角距离，即`坐标原点变换为了格子的左上角`了，这样就和图片无关了。这里注意一点，这个转换后的(x,y)仍然是中心点，只是坐标系变为了原点为格子左上方的。下面loss里面会讲这个地方。

## 损失的计算  

损失的计算分为有目标损失和无目标损失，先说无目标损失。无目标损失也就是这个格子里面不包含目标的中心点，也就是对应的`S*S*25`中25里面的第4个元素数值应该为0，这时候我们先把这样的格子取出来，这里使用了mask的思想，将对应位置元素mask出来。之后把预测的和label做mse损失即可。对于有目标的，我们也是先用mask将有目标的格子给mask出来，但是有目标的我们要分为三个损失，第一个和上面无目标的一样，也就是目标损失，将预测的和label的第四个元素做mse即可。第二个是bbox的损失，我们首先将bbox的值拿出来，也就是第0-4的元素，然后也是做mse损失。最后是分类损失，这个需要把第10-25的元素拿出来，做交叉熵损失即可。  

上面在说构造数据集的时候，我们说把(x,y)变为了相对格子左上角的，所以如果对于有些高级的yolo，每个格子可能会负责多个目标，这里需要计算一下iou，在计算iou的时候，需要拿到目标的左上角坐标，这个时候，我们直接用(x,y)-0.5(w,h)既可以了，因为在构造数据集的时候，虽然x，y变了，但其只是换了坐标系，其代表的仍然是中心坐标。 

## 总结 

这个介绍仅仅是一个简单版本，里面还有些概念，比如iou以及一个格子负责多个时，还有最后预测的nms都没有涉及到，以后有机会会进行一下扩充。 

