所有mv命令都可以用cp命令加上rm命令完成

移动文件


Linux下拷贝一个目录：
比如要把/home/user拷贝到/mnt/temp
cp -r /home/user/* /mnt/temp
但是这样有一个问题，/home/usera下的隐藏文件都不会被拷贝，子目录下的隐藏文件倒是会的。
正确方法：
cp -r /home/user/. /mnt/temp

