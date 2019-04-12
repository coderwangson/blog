# tensorflow加载npy文件 tf.py_func

标签： `Tensorflow`

---

这两天做一个小项目，里面我把数据集处理后把每个处理后的样本都保存成了`.npy`，但是等到我使用的时候忽然没办法读取，因为之前的读取图片我们在读取后可以使用`decode_jpeg`，读取二进制可以使用`decode_raw`，但是对于npy或者其他任意格式的文件却不知道怎么用tf读取，对于npy文件，如果直接使用python读取，简直轻松，我们使用`np.load`即可，所以有了这个关联，便想到在tensorflow中是否也有对应的直接调用python方法的方法呢，后来果然找到了，那就是`tf.py_func`，我们通过这个就可以直接传入一个使用python原生方法，然后包装成tensor。我在这里是这样用的：    

```python
def read_npy_file(item):
    data = np.load(item.decode())
    return data.astype(np.float32)
file_queue = tf.train.slice_input_producer([file_list,label_list])
image_contents = tf.py_func(read_npy_file,[file_queue[0]],[tf.float32,])
```  

其中read_npy_file就是在python读取npy的方法，file_list就是所有文件命名格式如`["1.npy","2.npy"...]`，构造出来了一个文件名队列,关键代码是`image_contents = tf.py_func(read_npy_file,[file_queue[0]],[tf.float32,])`，我觉得这句话作用就先当于读取了npy里面的东西，并且封装成一个tensor，这样我们后面才能构造成batch之类的，开启文件队列。   

有了这个方法以后我们就可以自定义解码自己的文件了，你想要读什么都可以，而不需要局限于tf提供的那几个解码器或者文件读取器。  

参考：  

https://stackoverflow.com/questions/48889482/feeding-npy-numpy-files-into-tensorflow-data-pipeline


