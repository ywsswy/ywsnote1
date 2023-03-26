grub rescue>ls
列出所有分区
grub rescue>ls (hd0,msdos1)
回车键，如果是unknown filesystem继续试下一个分区
grub rescue>ls (hd0, msdos8)
找到了,我的ubuntu是在（hd0,msdos8）
修改启动分区：
grub rescue>root=(hd0,msdos8)
grub rescue>prefix=/boot/grub
grub rescue>set root=(hd0,msdos8)
grub rescue>insmod normal
grub rescue>normal(进入启动菜单)
按C进入命令行模式：
grub>set root=hd0,msdos8
grub>set prefix=(hd0,msdos8)/boot/grub
grub>linux /vmlinuz  root=/dev/sda8
grub>initrd /initrd.img
grub>boot
进入ubuntu修复grub:
sudo update-grub
sudo grub-install /dev/sda //重建grub到第一个硬盘mbr
重启，OK!

【others
halt 关机
