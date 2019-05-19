# sklearn 决策树可视化

标签： `sklearn`

---

这里以iris分类问题为例子，主要分为三步，分别为得到模型，得到默认的树结构，导出为图片或者pdf。

## 得到决策树模型   

这个就是普通决策树例子，直接用skleran中的决策树分类器，然后喂入数据即可。  

```python
from sklearn.datasets import load_iris
from sklearn import tree
iris = load_iris()
clf = tree.DecisionTreeClassifier()
clf = clf.fit(iris.data, iris.target)
```  

这样得到的clf就是一个分类模型。  

## 导出默认的树结构  

这个是通过sklearn中tree的默认方法，可以得到一个dot文件（一个普通文本文件，保存树结构），这个文件可以保存也可以不保存，这里不保存，直接返回到一个变量里面，下面导出图片要用到。  
```python  

dot_data = tree.export_graphviz(clf, out_file=f,
                         feature_names=iris.feature_names,
                         class_names=iris.target_names,
                         filled=True, rounded=True,
                         special_characters=True)

```  

##导出图片或者pdf  

这个需要第三方工具，首先安装`Graphviz`，这是一个软件，通过[这个地址][1]下载，然后用`pip `或者`conda`安装`pydotplus`。  

通过下面的代码从dot里面导出png。  

```python
import pydotplus
import os
os.environ["PATH"] += os.pathsep + 'D:/Program Files (x86)/Graphviz2.38/bin/'
graph = pydotplus.graph_from_dot_data(dot_data)
graph.write_png("tree.png")
```  

如果出现`# pydotplus.graphviz.InvocationException: GraphViz's executables not foundpydotplus.graphviz.InvocationException: GraphViz's executables not found`一般就是GraphViz没有安装成功，或者路径导入错误，注意`os.environ["PATH"]`后面的路径是你安装Graphviz的路径。  

参考：  

https://www.jianshu.com/p/dd552f780a40 

https://blog.csdn.net/pilipala6868/article/details/79963650  

https://blog.csdn.net/llh_1178/article/details/78516774


  [1]: http://www.graphviz.org/download/