|---|压缩|解压|ps|eg|
|---|---|---|---|---|
|*.tar|tar cvf|tar xvf|这个仅仅是多文件打包，并没有压缩|tar -cvf jpg.tar *.jpb #所有的jpg打包成jpb.tar|
|\*.tar.gz/\*.tgz|tar czvf|tar xzvf|这个才是打包并且压缩|tar -zxf VMwareTools-10.1.6-14329.tar.gz -C ~/myfolder/ ## -C是解压到某目录而非当前目录|
|*.gz|gzip|gzip -d|压缩gzip \<input> -c >\<output> <br> 解压gzip -d|
|*.tar.xz|-|tar xJvf|
|*.xz|-|xz -d|
|*.tar.bz2|-|tar xjvf|bzip2|
|*.rar|-|unrar e|
|*.zip|zip -r \<ziped_name> \<file>|unzip|

总结：
c和x对应
z表示有gzip压缩方法，J表示xz压缩方法，j表示bzip2压缩方法
v表示可视
f后跟文件名