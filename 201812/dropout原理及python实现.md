#  dropout原理及python实现

标签： 神经网络 python 

---

## dropout引入   

我们都知道在训练神经网络的时候，对于神经网络来说很容易产生过拟合现象，在解决神经网络的过拟合的时候，我们可以使用正则化进行防止过拟合现象的产生，除此之外我们也可以使用dropout来防止过拟合现象。  

dropout如它的名字一样，就是在进行传播的时候删除一些结点，这样的话我们就可以降低网络的复杂性，这样的话我们的网络就不会变的过拟合了。   

## dropout的实现  

dropout主要在于在进行网络传播的时候随机关闭一二写神经元，这里的关键就在于如何关闭，我们可以把神经元的输出乘0进行关闭，因为置位0后，这个神经元就不会起到作用了。具体步骤如下：  

1、建立一个维度与本层神经元数目相同的矩阵$D^{[l]}$.
2、根据概率(keep_prob)我们把$D^{[l]}$中的元素设置为0或者1.
3、把本层的激活函数的输出$a^{[l]}$ 与$D^{[l]}$相乘（对应元素乘）作为新的输出。  
4、把新的输出除以keep_prob(这一步不知道原因)。  
5、在进行反向传播的时候同样进行这样的操作，所以要把D存储下来，这样才能对被关闭的神经元对应的w不进行更新。    

## python代码实现 （主要是前向传播以及反向传播） 

    # 前向传播，需要用到一个输入x以及所有的权重以及偏执项，都在parameters这个字典里面存储
    # 最后返回会返回一个caches里面包含的 是各层的a和z，a[layers]就是最终的输出
    def forward(x,parameters,keep_prob = 0.5):
        a = []
        z = []
        d = []
        caches = {}
        a.append(x)
        z.append(x)
        # 输入层不用删除
        d.append(np.ones(x.shape))
        layers = len(parameters)//2
        # 前面都要用sigmoid
        for i in range(1,layers):
            z_temp =parameters["w"+str(i)].dot(a[i-1]) + parameters["b"+str(i)]
            a_temp = sigmoid(z_temp)
            # 1、建立一个维度与本层神经元数目相同的矩阵$D^{[l]}$.
            d_temp = np.random.rand(z_temp.shape[0],z_temp.shape[1])
            #2、根据概率(keep_prob)我们把$D^{[l]}$中的元素设置为0或者1.
            d_temp = d_temp < keep_prob
            3、把本层的激活函数的输出$a^{[l]}$ 与$D^{[l]}$相乘（对应元素乘）作为新的输出。4、除keep_prob
            a_temp = (a_temp * d_temp)/keep_prob
            z.append(z_temp)
            a.append(a_temp)
            d.append(d_temp)
            
        # 最后一层不用sigmoid,也不用dropout
        z_temp = parameters["w"+str(layers)].dot(a[layers-1]) + parameters["b"+str(layers)]
        z.append(z_temp)
        a.append(z_temp)
        d.append(np.ones(z_temp.shape))
        
        caches["z"] = z
        caches["a"] = a
        # 记得保存起来，因为反向传播还会使用
        caches["d"] = d
        caches["keep_prob"] = keep_prob    
        return  caches,a[layers]   
        
        
    # 反向传播，parameters里面存储的是所有的各层的权重以及偏执，caches里面存储各层的a和z
    # al是经过反向传播后最后一层的输出，y代表真实值 
    # 返回的grades代表着误差对所有的w以及b的导数
    def backward(parameters,caches,al,y):
        layers = len(parameters)//2
        grades = {}
        m = y.shape[1]
        # 假设最后一层不经历激活函数
        # 就是按照上面的图片中的公式写的
        grades["dz"+str(layers)] = al - y
        grades["dw"+str(layers)] = grades["dz"+str(layers)].dot(caches["a"][layers-1].T) /m
        grades["db"+str(layers)] = np.sum(grades["dz"+str(layers)],axis = 1,keepdims = True) /m
        # 前面全部都是sigmoid激活
        for i in reversed(range(1,layers)):
            da_temp = parameters["w"+str(i+1)].T.dot(grades["dz"+str(i+1)])
            5、在进行反向传播的时候同样进行这样的操作，所以要把D存储下来，这样才能对被关闭的神经元对应的w不进行更新
            # 要记得乘上对应的开关caches["d"][i]，这样就保证了关闭的神经元在反向传播的时候仍然是关闭的
            da_temp = (caches["d"][i] * da_temp)/caches["keep_prob"]
            grades["dz"+str(i)] = da_temp * sigmoid_prime(caches["z"][i])
            grades["dw"+str(i)] = grades["dz"+str(i)].dot(caches["a"][i-1].T)/m
            grades["db"+str(i)] = np.sum(grades["dz"+str(i)],axis = 1,keepdims = True) /m
        return grades   
        
        
整个网络的其他部分代码可以参考：https://blog.csdn.net/qq_28888837/article/details/84296673   

参考:
>https://blog.csdn.net/qq_28888837/article/details/84296673  
https://zhuanlan.zhihu.com/p/29592806









