# Ubuntu 挂载移动硬盘-NTFS格式

标签： `Ubuntu`

---

单纯的Ubuntu挂载移动硬盘，网上有很多的教程，但我的是多个分区的并且是NTFS格式的，似乎没找到教程，后来自己摸索出来了，所以记录一下。  



首先，把移动硬盘插入服务器（装的Ubuntu系统）。然后用`sudo -s`获取root权限。  

1. 输入`fdisk -l` 查看硬盘信息，你可以根据大小判断哪个盘是你的移动硬盘  

![2019-10-23_170745.jpg](http://ww1.sinaimg.cn/large/005Dd0fOgy1g888wo8putj30p30c17dn.jpg)  

2.  从上面可以看出我的500G的移动硬盘已经插入到服务器的`/dev/sdb`上，如果按照常规，直接输入`mount /dev/sdb ~/mnt/`就完事了，但是因为我的是NTFS格式的，所以会报一个
**mount: you must specify the filesystem type**的错误，所以通过`mount -t ntfs-3g  /dev/sdb ~/mnt/` 指定格式挂载，其中`ntfs-3g`在新版的Ubuntu上面是有的，如果没有你可以自行安装。  

**但** 事情往往没有那么简单，又报错了  

```
Failed to mount '/dev/sdb': Invalid argument
The device '/dev/sdb' doesn't seem to have a valid NTFS.
Maybe the wrong device is used? Or the whole disk instead of a
partition (e.g. /dev/sda, not /dev/sda1)? Or the other way around?
```  

这个原因我猜和我分区了有关，因为我的硬盘500G是分了3个区，所以无法直接把整个硬盘挂起来。  

3. 通过`parted -l`查看所有分区信息：   

```
Number  Start   End     Size    Type      File system     Flags
 1      1049kB  500MB   499MB   primary   ext4            boot
 2      501MB   2000GB  2000GB  extended
 5      501MB   8693MB  8191MB  logical   linux-swap(v1)
 6      8694MB  213GB   205GB   logical   ext4
 7      213GB   2000GB  1787GB  logical   ext4


Model: Seagate Expansion (scsi)
Disk /dev/sdb: 500GB
Sector size (logical/physical): 512B/4096B
Partition Table: gpt

Number  Start   End     Size    File system  Name                  Flags
 1      1049kB  105MB   104MB   fat32        EFI system partition  boot
 2      105MB   239MB   134MB                MSR partition         msftres
 3      239MB   34.6GB  34.4GB               Basic data partition  msftdata
 4      34.6GB  172GB   137GB   ntfs         Basic data partition  msftdata
 5      172GB   500GB   328GB   ntfs         Basic data partition  msftdata

```  

可以看到下面的5个分区就是我的（1、2是隐藏的，不用管），其中3-5就是我的三个分区。  

所以我该怎么挂载呢，因为没有对应的设备号。我就急中生智，直接把这个`3 4 5`放在sdb后面应该就行了，所以尝试`mount -t ntfs-3g  /dev/sdb4 ~/mnt/` ,果然挂载成功。  





