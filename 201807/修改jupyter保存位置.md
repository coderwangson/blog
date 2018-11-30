## 修改jupyter保存位置  

这个问题在网上找了好久，很多方案都只是修改那个py配置文件，但是对于我的电脑来说修改了也不行，后来发现还要修改那个Jupyter Notebook的属性。  

### 1. 产生并且修改配置文件  

命令行下面运行	jupyter notebook --generate-config 命令，这样会在你的硬盘 C:\Users\Administrator\.jupyter 下产生一个配置文件jupyter_notebook_config.py。  

然后打开该文件（用记事本就可以），查找到c.NotebookApp.notebook_dir，然后在后面写上你想要的位置，注意前面 **#要删除**。下面是我修好后的样子。  

![](https://i.imgur.com/jtfcMZc.png)


### 2. 修改jupyter  

对于有些人可能上面修改完就ok了，但可能对于有些人来说还需要再进行一步。  

你右键Jupyter的快捷方式（在开始菜单的Anaconda下面），然后选择属性，此时你把%USERPROFILE%删除即可。如果删除后还不行，就把%USERPROFILE%这个位置写成你上面配置文件中的路径。  

### 参考  

[https://stackoverflow.com/questions/35254852/how-to-change-the-jupyter-start-up-folder/36433389#36433389](https://stackoverflow.com/questions/35254852/how-to-change-the-jupyter-start-up-folder/36433389#36433389)


