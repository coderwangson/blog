# Secure Face Unlock: Spoof Detection  on Smartphones   

标签： `论文` `spoofing` 

---

论文出处：IEEE TRANSACTIONS ON INFORMATION FORENSICS AND SECURITY, VOL. 11, NO. 10, OCTOBER 2016   


## 本文提出的方法  

在引言中介绍了当前的2D欺诈检测的比较好的方法，并且给出了表格：    

![image](http://wx4.sinaimg.cn/large/005Dd0fOly1g3qjcluw74j309006yjse.jpg)


还介绍了当下流行的公共数据库：     

![image](http://wx2.sinaimg.cn/large/005Dd0fOly1g3qjcwocplj309n04y74v.jpg)


本文的主要贡献是提出了一个新的数据库MSUUSSA，混合多种特征的特征表征，研究了两种拒绝的选项--IPD（图片的展示距离）和边框（因为有些欺诈可能有边框，可直接判假），研究了在智能手机上的应用以及交叉库的结果。  

为了找到多个具有区分性的特征，本文研究了镜面反射，颜色分布，摩尔图案，图片扭曲几中具有区分性的图形特征。并且研究了一些特征表示符以及结果（对于单个面部）：    
![image](http://wx1.sinaimg.cn/large/005Dd0fOly1g3qjd7z07aj307g0a6abf.jpg)   

通过分析发现不同的攻击方案可能适合于不同的特征描述符，所以最终使用多个特征混合：LBP+colorMoment的方案。除此之外还给出了两个拒绝选项-IPD以及边框，IPD是根据面部在图片中的占比（因为欺诈图片可能为了避免出现摩尔现象或者边框所以其距离或者过小（避免边框）或者过大（避免摩尔）），所以可以要求一个阈值，这样在智能手机上面运行的时候，根据这个阈值也能过滤掉一部分。所以最后的架构如下：  

![image](http://ws1.sinaimg.cn/large/005Dd0fOly1g3qjde6upaj309903i759.jpg)  

在单个库上的结果看TableIII，交叉库的结果如下：  

![image](http://wx3.sinaimg.cn/large/005Dd0fOly1g3qjduojiuj308k056jsc.jpg)

## 收获  

1、可以考虑做具体方案，比如本文做的就是适合手机的。  


## 参考文献重点摘录可作为以后读  

3D信息的：  

[9] M. De Marsico, M. Nappi, D. Riccio, and J.-L. Dugelay, “Moving 
face spoofifing detection via 3D projective invariants,” in Proc. ICB, 
Mar./Apr. 2012, pp. 73–78. 

基于图像失真的：  

[20] D. Wen, H. Han, and A. K. Jain, “Face spoof detection with image 
distortion analysis,” IEEE Trans. Inf. Forensics Security, vol. 10, no. 4, 
pp. 746–761, Apr. 2015.







