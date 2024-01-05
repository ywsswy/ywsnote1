## df -Th 看到的是已经挂载（mount）好的所有“文件系统”(mac上结合diskutil list使用)的磁盘使用情况

## 但是如果新分配的磁盘（block storge）还查不到，只能在fdisk -l中看所有的“磁盘”（lsblk也能看到所有磁盘及大小，df -Th只是文件系统的大小）
输出形如：
```
Disk <磁盘名>: <磁盘大小> GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xf6abafec

   Device Boot      Start         End      Blocks   Id  System
<分区名（通常是磁盘名后面跟个数字）>              63   209712509   104856223+  83  Linux

……
```

## 其中新磁盘就没有分区，要创建分区：
1.进入磁盘 fdisk <磁盘名>
2.新分一个区，输入命令：n
3.默认选择主分区类型，输入命令：p
4.选择分区号，默认1，使用默认的即可。输入命令：回车键
5.选择起始扇区，根据需要选择，默认为起始位置。输入命令：回车键或输入位置
6.选择末尾扇区，根据需要选择，默认为最后位置。输入命令：回车键或输入位置
7.保存分区表，输入命令：w

## 然后格式化分区，linux文件系统是：mkfs -t ext4 <分区名>

## 最后挂载到文件系统中 mount <分区名> <挂载点也即目标目录（必须要存在）>


## 其他：
- 图形界面自动挂载相当于
sudo mkdir /media/hill/myCdisk
sudo mount /dev/sda5 /media/hill/myCdisk

- 卸载
sudo umount /media/hill/myCdisk

- 如果卸载失败，可以查看是什么占用
fuser -m -v

- U盘文件损坏，先查看/var/log/syslog确诊 然后sudo umount /media/hill/AC 最后sudo dosfsck -v -a /dev/sdb1修复
