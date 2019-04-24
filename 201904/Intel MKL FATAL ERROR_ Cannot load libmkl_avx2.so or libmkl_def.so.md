# Intel MKL FATAL ERROR: Cannot load libmkl_avx2.so or libmkl_def.so

标签： `linux` `python`

---

这个错误是我导入caffe的时候出错的，但是这个问题似乎是因为`mkl`的原因，之前在网上有人推荐在`.bashrc`文件中添加如下命令：  

`export LD_PRELOAD=libmkl_rt.so` 虽然问题解决了，但是每次总会有一些警告，所以觉得不好。后来又看到有人是通过安装nomkl解决的，所以就试了试，如果使用的conda，则直接通过`conda nomkl`即可解决问题，并且还没有警告。  

参考：  

https://stackoverflow.com/questions/36659453/intel-mkl-fatal-error-cannot-load-libmkl-avx2-so-or-libmkl-def-so





