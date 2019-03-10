# Tensorflow 文件读取

标签： `Tensorflow`

---

在我们进行数据训练前，我们都需要先进行数据的读取，我们的数据可能是文本文件，csv文件，或者二进制文件以及图片文件。这些都在tensorflow中提供了对应的api。并且由于tensorflow需要把主要工作集中在计算上，所以不应该先进行数据读取，再进行计算，因为可能数据很大，所以读取要很长时间，所以可以采取多线程的方式进行管理，这样就可以增加效率。  

## Tensorflow读取数据的步骤  

* 创建文件名队列（这样tensorflow就知道从哪里读取文件了）
* 读取文件内容
* 对内容解码
* 批处理
* 会话里面开启进程

对应api：  

* file_queue =tf.train.string_input_producer(filelist)可以构造文件名队列，参数是一个文件名list
* reader = tf.TextLineReader()或者tf.FixedLengthRecordReader()等可以构造一个读取器，然后使用reader.read(file_queue)就可以读文件了，<b>注意每次只读一个样本</b>.
* tf.decode_csv(value）这个可以对value按照格式解码  
* tf.train.batch可以进行批处理，传入一个上面的解码后的数据，然后就可以制造一个批次。
* thread = tf.train.start_queue_runners(sess,coord)开启线程  

其中有几个地方要说的是，在读取的时候每次只读一个样本，并且batch的时候也是传入的一个样本，但是batch返回的是多个样本。而coord = tf.train.Coordinator()代表定义了一个协调器，通过这个协调器能关闭线程。    

## 代码  

```python  

import tensorflow as tf
from glob import glob
def csvread(filelist):
    # 构造文件的队列
    file_queue = tf.train.string_input_producer(filelist)
    # 构造文件读取器 并从文件名队列读

    reader = tf.TextLineReader()
    key,value = reader.read(file_queue) #key 是文件名，value是具体的值 每次读一个样本

    # 对内容解码
    record = [[""],[""]] # 二维，每个[]里面代表了数据的格式 以及默认值 比如写了1 代表整型默认为1
    feature = tf.decode_csv(value,record_defaults=record) # 返回所有特征

    # 批处理
    batch = tf.train.batch(feature,batch_size=2,num_threads=2,capacity=10)

    return batch
if __name__ == '__main__':

    filelist = glob("./data/*.csv")
    batch = csvread(filelist)
    # 定义线程协调器
    coord = tf.train.Coordinator()
    # 开启会话
    with tf.Session() as sess:

        # 开启线程
        thread = tf.train.start_queue_runners(sess,coord) # 直接开启即可，不用像以前那样自己创建线程了
        print(sess.run(batch))
        coord.request_stop()
        coord.join()
```






