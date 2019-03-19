# Bob库安装

标签： `python`

---

因为做欺诈检测，有些论文用的是这个库，所以就尝试着装了一下，注意的是这个库要用翻墙才可以，如果校园网的话可以直接使用。  

[它的官网][1]，我参考的是里面安装的这个目录https://www.idiap.ch/software/bob/docs/bob/docs/stable/bob/doc/install.html。  

其实上面给的比较全，几乎按照上面的方法就行，先创建一个独立的conda环境，然后设置源，然后开始装，但是这样还要一个个库安装，所以我采用的是下面的方式，通过ymal文件安装。直接在官网下载对应版本的ymal文件，然后按照命令安装即可，非常简单，但是唯一注意就是只能在`Linux`上并且要翻墙，所以如果这两个前提不具备，就别装了。如果你装好的话，你会发现真的好用，上面装了`cv、caffe、tensorflow`，非常全。


  [1]: https://www.idiap.ch/software/bob/