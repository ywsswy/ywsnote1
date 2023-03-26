所有mv命令都可以用cp命令加上rm命令完成

移动文件


Linux下拷贝一个目录：
比如要把/home/user拷贝到/mnt/temp
cp -r /home/user/* /mnt/temp
但是这样有一个问题，/home/usera下的隐藏文件都不会被拷贝，子目录下的隐藏文件倒是会的。
正确方法：
cp -r /home/user/. /mnt/temp


linux报错解释：
```
[root@VM-184-145-centos tmp]# mv new.py pl/new.py
mv: cannot move 'new.py' to 'pl/new.py': No such file or directory
# 这一种是因为pl目录不存在
[root@VM-184-145-centos tmp]# mv old.py plugin/new.py 
mv: cannot stat 'old.py': No such file or directory
# 这一种是因为old.py文件不存在
```