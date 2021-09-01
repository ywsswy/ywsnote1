图形界面自动挂载相当于
sudo mkdir /media/hill/myCdisk
sudo mount /dev/sda5 /media/hill/myCdisk

卸载
sudo umount /media/hill/myCdisk


# 如果卸载失败，可以查看是什么占用
fuser -m -v


## 其他
U盘文件损坏，先查看/var/log/syslog确诊 然后sudo umount /media/hill/AC 最后sudo dosfsck -v -a /dev/sdb1修复


df -h     　　　　#显示目前在Linux系统上的文件系统的磁盘使用情况统计。Filesystem 文件系統 Mounted on 挂载的目录
lsblk    　　　　#列出块设备信息（df -h不能看到的卷）