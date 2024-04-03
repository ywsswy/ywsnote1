|---|压缩|解压|ps|eg|
|---|---|---|---|---|
|*.tar|tar cvf|tar xvf|这个仅仅是多文件打包，并没有压缩|tar -cvf jpg.tar *.jpb #所有的jpg打包成jpb.tar|
|\*.tar.gz/\*.tgz|tar czvf|tar xzvf|这个才是打包并且压缩|tar -zxf VMwareTools-10.1.6-14329.tar.gz -C ~/myfolder/ ## -C是解压到某目录而非当前目录|
|*.gz|gzip<br>gzip -c|gzip -d<br>gunzip（这个要求必须以gz后缀结尾）|压缩（不保留原始文件，文件名直接变成.gz）gzip \<file_name><br>压缩（保留原始文件）gzip -c \<file_name> >\<output><br>gzip -1 使用一级别压缩 压缩比例最少 压缩速度最快 -9 压缩比例最大 压缩速度最慢 默认1-9 不加级别默认是6级别|
|*.tar.xz|-|tar xJvf|
|*.xz|-|xz -d|
|*.tar.bz2|-|tar xjvf|bzip2|
|*.rar|-|unrar e|
|*.zip|zip -r \<ziped_name> \<file>|unzip|
|?|\|zlib-flate -compress|\|zlib-flate -uncompress|从标准输出读数据，这个工具是qpdf包中的|

总结：
c和x对应
z表示有gzip压缩方法，J表示xz压缩方法，j表示bzip2压缩方法
v表示可视
f后跟文件名

额外加密解密：
openssl enc -des3 -in 1.tgz -e >1.tgz.encrypt # 加密，等价于openssl des3 -in 1.tgz -e >1.tgz.encrypt
openssl enc -des3 -in 1.tgz.encrypt -d >1.tgz # 解密
