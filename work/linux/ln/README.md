硬链接
ln old new 不能对目录创建
ls -lai 显示的是文件，能看到每个文件名所对应的inode号及该inode有多少个硬链接
rm一个文件名，是对应inode减1，减到0才真正删除一个文件。 


软链接/符号链接
ln -s old new   #old最好写绝对路径 -sf是强制覆盖旧链接
ln -lai显示的是链接，符号链接本身是一个新的inode