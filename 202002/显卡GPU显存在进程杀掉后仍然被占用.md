# 显卡/GPU显存在进程杀掉后仍然被占用

可以使用`fuser -v /dev/nvidia*`查看占用显存的进程，一般这些进程都是读取数据的，所以即使把你的python杀掉，这些仍然在后台运行。  

由于数目过多，可以使用下面的批处理语句进行处理：  

fuser -v /dev/nvidia* |awk '{for(i=1;i<=NF;i++)print "kill -9 " $i;}'