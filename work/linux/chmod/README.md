[drwxrwxrwx][不允许处为-]
[r w x][分别代表read=4 write=2 execute=1]，三组限制依次是[owner/group/other]，对root用户来说是不受任何权限限制的
-rwxrw-r‐-1 root root 1213 Feb 2 09:39 abc
第一个字符代表文件（-）、目录（d），链接（l）
chmod 755 abc：给文件abc权限rwxr-xr-x
chmod u-x abc：给abc文件去除用户执行的权限，增加组写的权限
# 文件
- x执行权限 可以./的方式执行
- 就算 chmod000了，但是所属者依然可以重新chmod
- 默认新建的文件是 rw r r

# 目录
默认新建文件夹是@drwxr-xr-x user:user
- r可读仅仅能ls目录
- w可以重命名删除此文件夹内的东西"仅仅管一层，两层的深度就管不到了"
- x仅仅能cd进来（如果有某个祖宗目录没有x权限，应该？也进不来）

# 安全的管理方式
find . -type f -exec chmod o-rwx,g-wx {} \;
find . -type d -exec chmod o-rwx,g-rw {} \;

# 修改文件夹下所属用户和组
chown -R <user>:<group> <folder>