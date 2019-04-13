# TSNE 高维数据可视化

标签： `python` `机器学习` `神经网络`

---

在神经网络中，我们最后一层一般都是高纬度的数据，但是有时候我们可能想看一下这些高纬度数据的分布情况，这个时候就需要用TSNE，其实TSNE本质上就是先利用PCA降维，比如降到二维或者三维，做法就是把神经网络某层的特征降维，然后映射到二维或者三维空间可视化。这样就能看到各层特征分布情况。  

## TSNE降维  

```python
from sklearn.manifold import TSNE
tsne = TSNE(perplexity=30, n_components=2, init='pca', n_iter=5000) # TSNE降维，降到2
# 只需要显示前500个
plot_only = 500
# 降维后的数据
low_dim_embs = tsne.fit_transform(last_layer.data.numpy()[:plot_only, :])
# 标签
labels = by.numpy()[:plot_only]
plot_with_labels(low_dim_embs, labels,"epoch{}_step{}".format(e,step))
```
这里用的数据是之前pytroch进行mnist中的，last_layer代表最后一层特征，这里降到了2维，by代表一个批次的y,为了后面可视化可以区分，`plot_with_labels`是一个自定义方法，用来可视化降维后的数据。  

## 可视化   

```python

from matplotlib import cm
def plot_with_labels(lowDWeights, labels,i):
    plt.cla()
    # 降到二维了，分别给x和y
    X, Y = lowDWeights[:, 0], lowDWeights[:, 1]
    # 遍历每个点以及对应标签
    for x, y, s in zip(X, Y, labels):
        c = cm.rainbow(int(255/9 * s)) # 为了使得颜色有区分度，把0-255颜色区间分为9分,然后把标签映射到一个区间
        plt.text(x, y, s, backgroundcolor=c, fontsize=9)
    plt.xlim(X.min(), X.max())
    plt.ylim(Y.min(), Y.max()); plt.title('Visualize last layer')

    plt.savefig("{}.jpg".format(i))
```

可视化就是把前面降维过的数据画到二维空间，这里的关键点在于同一类别的用同种颜色画，体现在代码`c = cm.rainbow(int(255/9 * s))`以及`backgroundcolor=c`上面，这样的话如果两个点的标签同，那么绘制的颜色也是一样的。 

 
 下图是一个epoch后的分布情况可以看出数据已经很有区分性了：

![image](https://ws3.sinaimg.cn/large/005Dd0fOly1g20s04idovj30hs0dc3zk.jpg)




