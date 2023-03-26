# write the following command in ~/.bashrc
export PATH=<the path you what to push into $PATH>:$PATH
# execute source ~/.bashrc to take effect immediately.

# 每次ssh连接到服务器会先执行.profile文件和.bashrc文件
# 每次打开一个新bash都会执行.bashrc
# 综上写到.profile是好的