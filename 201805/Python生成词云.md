## Python生成词云

### 1.1 准备的相关包
PIL、numpy、matplotlib以及wordcloud，其中前三个我都是通过pip install的方式安装的，但是到了wordcloud发现安装总是失败，原因是缺少vc++14.0，后来我去wordcloud的官网上看，发现还可以通过conda的方式进行安装，所以我就换了中方式，就是用了conda的方式。
#### 1.1.1 conda安装
这个我也是第一次，所以大部分都是边查资料，边配置，通过这次的实践，我觉得conda就相当于一个管理器，可以通过它安装一些包，甚至安装python。  
conda安装步骤：  
1、先安装conda客户端。  
2、安装后就可以通过命令行方式进行使用了
![](https://i.imgur.com/VEzcfSL.png)
3、然后就在conda中安装python环境，conda create --name python36 python=3.6，这样就会在你的conda的envs目录下安装一个python，这样在conda的黑窗口中就可以使用python了。
![](https://i.imgur.com/1KHpKuD.png)

4、安装完python，就可以使用conda安装wordcloud了，对于其他三个，你既可以用conda，也可以pip方式安装。（我好像没安装，好像conda自己带的有）安装wordcloud的命令 conda install -c conda-forge wordcloud
附：关于详细的conda的应用请自己学习一下，我并不是很了解，这里只是拿来安装了个东西，所以可能有些解释不是很准确。
### 2. 代码编制
有了上面的基础，这个环境就搭建起来了，剩下的就是代码的编制了。因为是生成词云，就是很多个词，然后根据词的出现频率组成一个特定形状，所以这里要准备两个东西，一个是词语以及频率，还有就是生成的图案。
这里词语级频率使用文件存储。，图像随意。如下：
![词语](https://i.imgur.com/TT7ZLD2.png)
![](https://i.imgur.com/Sz58THm.png)

有了这两个东西，剩下的就是把二者关联在一起，这里需要用到两个技术，一个就是读文件，另个就是生成词云。
#### 2.1 读文件
	keywords=dict()
	f=open('D:\\a.txt','r')
	for i in f:
	    k=i.split(":")
	    keywords[k[0]]=float(k[1][0:len(k[1])-1])

上面是读文件的代码，其中float(k[1][0:len(k[1])-1])这个中k[1][0:len(k[1])-1]代表截取字符串，之所以要这样做的原因是因为我的数据格式有问题，你也可以不这样做，因为这个词语和频率的数据来源可以任意，只要最后生成一个dict的keywords就行了。最终keywords是{"xx"：0.2，“uu”:0.3}这个样子。
#### 2.2 生成词云
这一步相对比较简单，因为都是使用别人的封装好的方法而已。代码如下：  

    # 读原图
	image= Image.open('D:\\tim.png')
    # 下面都是一些基本设置，感兴趣可以搜一下
	graph = np.array(image)
	wc = WordCloud(font_path='./fonts/simhei.ttf',background_color='White',max_words=50,mask=graph)
	wc.generate_from_frequencies(keywords)
	image_color = ImageColorGenerator(graph)
	plt.imshow(wc)
	plt.imshow(wc.recolor(color_func=image_color))
	plt.axis("off")
	plt.show()
    # 写词云图片到硬盘
	wc.to_file('D:\\dream.png')


### 3. 全部代码

```

	#encoding=utf-8
	from PIL import Image,ImageSequence
	import numpy as np
	import matplotlib.pyplot as plt
	from wordcloud import WordCloud,ImageColorGenerator
	keywords=dict()
	f=open('D:\\a.txt','r')
	for i in f:
	    k=i.split(":")
	    keywords[k[0]]=float(k[1][0:len(k[1])-1])
	# print(keywords)
	image= Image.open('D:\\tim.png')
	graph = np.array(image)
	wc = WordCloud(font_path='./fonts/simhei.ttf',background_color='White',max_words=50,mask=graph)
	wc.generate_from_frequencies(keywords)
	image_color = ImageColorGenerator(graph)
	plt.imshow(wc)
	plt.imshow(wc.recolor(color_func=image_color))
	plt.axis("off")
	plt.show()
	wc.to_file('D:\\dream.png')

```

记得导入相应包。
最终结果：
![](https://i.imgur.com/uA9cNgR.png)
### 4. 总结
这个程序其实不是很麻烦，因为是站在前人的肩膀上，但是前期安装wordcloud是花了大工夫，后来发现conda安装包真是方便，所以希望以后能学一下conda。




