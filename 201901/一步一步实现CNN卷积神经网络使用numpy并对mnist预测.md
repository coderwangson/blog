# 一步一步实现CNN卷积神经网络使用numpy并对mnist预测

标签： `CNN` 

---  

## 卷积神经网络介绍  

卷积神经网络其实是利用了数字图像处理中的卷积操作，因为卷积的强大，所以能用作一个强大的特征提取器，然后我们使用提取得到的特征连接到全连接层，这样会使得预测的结果比较准确。  

## 先搭建卷积层的模块  

卷积层主要有两个操作，一个是卷积操作，一个是池化操作。除此之外由于使用卷积直接进行操作可能比较耗时，因为好几层循环，所以我们可以使用im2col方法进行优化。下面给出代码：  

```python 
import numpy as np
import CNN_utils
# 带来im2col的
# ref ：https://blog.csdn.net/Daycym/article/details/83826222
class Convolution:
    # 初始化权重（卷积核4维）、偏置、步幅、填充
    def __init__(self, W, b, stride=1, pad=0):
        self.W = W
        self.b = b
        self.stride = stride
        self.pad = pad
        # 中间数据（backward时使用）
        self.x = None   
        self.col = None
        self.col_W = None
        # 权重和偏置参数的梯度
        self.dW = None
        self.db = None
    # 进行前向传播其实就是输出的卷积结果
    def forward(self, x):
        # 卷积核大小
        FN, C, FH, FW = self.W.shape
        # 数据数据大小
        N, C, H, W = x.shape
        # 计算输出数据大小
        out_h = 1 + int((H + 2*self.pad - FH) / self.stride)
        out_w = 1 + int((W + 2*self.pad - FW) / self.stride)
        # 利用im2col转换为行
        col = CNN_utils.im2col(x, FH, FW, self.stride, self.pad)
        # 卷积核转换为列，展开为2维数组
        col_W = self.W.reshape(FN, -1).T
        # 计算正向传播
        out = np.dot(col, col_W) + self.b
        out = out.reshape(N, out_h, out_w, -1).transpose(0, 3, 1, 2)

        self.x = x
        self.col = col
        self.col_W = col_W
        return out
    def backward(self, dout):
        # 卷积核大小
        FN, C, FH, FW = self.W.shape
        dout = dout.transpose(0,2,3,1).reshape(-1, FN)
        self.db = np.sum(dout, axis=0)
        self.dW = np.dot(self.col.T, dout)
        self.dW = self.dW.transpose(1, 0).reshape(FN, C, FH, FW)
        dcol = np.dot(dout, self.col_W.T)
        # 逆转换
        dx = CNN_utils.col2im(dcol, self.x.shape, FH, FW, self.stride, self.pad)
        return dx
    
class Pooling:
    def __init__(self, pool_h, pool_w, stride=1, pad=0):
        self.pool_h = pool_h
        self.pool_w = pool_w
        self.stride = stride
        self.pad = pad
        
        self.x = None
        self.arg_max = None

    def forward(self, x):
        N, C, H, W = x.shape
        out_h = int(1 + (H - self.pool_h) / self.stride)
        out_w = int(1 + (W - self.pool_w) / self.stride)
        # 展开
        col = CNN_utils.im2col(x, self.pool_h, self.pool_w, self.stride, self.pad)
        col = col.reshape(-1, self.pool_h*self.pool_w)
        # 最大值
        arg_max = np.argmax(col, axis=1)
        out = np.max(col, axis=1)
        # 转换
        out = out.reshape(N, out_h, out_w, C).transpose(0, 3, 1, 2)
        self.x = x
        self.arg_max = arg_max
        return out

    def backward(self, dout):
        dout = dout.transpose(0, 2, 3, 1)
        pool_size = self.pool_h * self.pool_w
        dmax = np.zeros((dout.size, pool_size))
        dmax[np.arange(self.arg_max.size), self.arg_max.flatten()] = dout.flatten()
        dmax = dmax.reshape(dout.shape + (pool_size,)) 
        dcol = dmax.reshape(dmax.shape[0] * dmax.shape[1] * dmax.shape[2], -1)
        dx = CNN_utils.col2im(dcol, self.x.shape, self.pool_h, self.pool_w, self.stride, self.pad)
        return dx

```  

这两步操作就包含了一个卷积一个池化。如果想了解卷积池化的具体操作可以参考[吴恩达的视频][1]以及[这个代码][2]，这个链接上的代码就是参考吴恩达的视频写的，但是效率不高，所以采用了上面的使用im2col方法的。  

## 定义多个卷积层  

```python  
# 卷积核
    W1 = np.random.normal(size = (6,1,5,5))
    b1 = np.zeros((1,1,1,6),dtype = np.float32)
    conv1 = Convolution2.Convolution(W1,b1,stride = 1,pad = 0)
    pool1 = Convolution2.Pooling(2,2,2)
    # 卷积核
    W2 = np.random.normal(size = (16,6,5,5))
    b2= np.zeros((1,1,1,16),dtype = np.float32)
    conv2 = Convolution2.Convolution(W2,b2,stride = 1,pad = 0)
    pool2 = Convolution2.Pooling(2,2,2)
```  

上面的代码定义了一个卷积层->池化层->卷积层->池化层这样的结构。  

## 把最后一个池化层进行平坦化并连接到全连接层  

经过最后一层池化操作其实我们已经得到了很多特征，然后我们需要做的就是把其进行平坦化(因为全连接需要的是二维数据，而卷积是三维的)，然后连接到我们的全连接层就行了，注意维度的变化。  

```python 
for i in range(60000):
        start = i*batch_size%train_y.shape[1]
        end = min(start+batch_size,train_y.shape[1])
        # x : 64 * 784 -> 64 * 1 * 28*28
        temp_train_x = train_x[start:end,].reshape((end-start,1,28,28))
        temp_train_y = train_y[:,start:end]
        #N, C, H, W = x.shape
        conv1_a  = conv1.forward(temp_train_x)
        pool1_a = pool1.forward(conv1_a)
        conv2_a = conv2.forward(pool1_a)
        pool2_a = pool2.forward(conv2_a)
        # 开始全连接层
        x = pool2_a.reshape((end-start,-1))
        x = x.T
        caches,al = forward(x, parameters,keep_prob=1)
        dx,grades = backward(parameters,caches, al, train_y[:,start:end])
        parameters = update_grades(parameters, grades,v, learning_rate= learning_rate)
        # 全连接结束
        #######
        # dx 相当于dA2
        dx = dx.T
        dx = dx.reshape(pool2_a.shape)
        dpool2_a = pool2.backward(dx)
        dconv2a = conv2.backward(dpool2_a)
        dpool1a = pool1.backward(dconv2a)
        dconv1a = conv1.backward(dpool1a)
        W2 -= learning_rate * conv2.dW
        b2 -=learning_rate*conv2.db
        W1 -= learning_rate * conv1.dW
        b1 -= learning_rate*conv1.db
```
上面需要说的有在把数据喂入到卷积层的时候，注意要把数据转成二维(因为我这里用的mnist是一个csv版的，所以读出来是一个二维数据)。然后就是在经过最后一个池化后，在连接全连接前，要进行把三维数据转化为二维，更新参数是先更新全连接的，然后更新到dx的时候相当于更新到了池化，然后再进行维度转化。  

## 全连接  
[简单的全连接可以参考这里][3]，复杂的全连接(带dropout等)代码可以参考[这里][4]

## 注意   
因为卷积操作和全连接的特性，所以我们在实现的时候卷积的数据一般是[batch_size,28,28]，但是全连接一般是[28*28,batch_size]，所以在这个地放要注意二者维度的区别，在计算的时候注意转换，如果你的程序没报错但是精度上不去，你就可以看看是不是数据转转置有问题，维度代表的意义不一样。  

全部代码可以在[该处][5]获得


  
参考：  

>  https://mooc.study.163.com/learn/2001281004?tid=2001392030#/learn/content?type=detail&id=2001728688

> https://blog.csdn.net/Daycym/article/details/83826222  

>


  [1]: https://mooc.study.163.com/learn/2001281004?tid=2001392030#/learn/content?type=detail&id=2001728688
  [2]: https://github.com/coderwangson/CNN/blob/master/Convolution.py
  [3]: https://blog.csdn.net/qq_28888837/article/details/84296673
  [4]: https://github.com/coderwangson/NN
  [5]: https://github.com/coderwangson/CNN