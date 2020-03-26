# opencv ImportError: libSM.so.6: cannot open shared object file: No such file or directory 

这个问题，网上的解决方案大多数都是apt-get安装libSM，但是由于我这里不方便使用包安装，所以发现可以使用`pip install opencv-python-headless` 安装无需图形依赖库的opencv。 

参考： https://stackoverflow.com/questions/47113029/importerror-libsm-so-6-cannot-open-shared-object-file-no-such-file-or-directo