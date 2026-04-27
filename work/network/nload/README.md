nload 查看incoming（下载速度）和outgoing（上传速度），看Curr参数就行，方向键切换网卡


wget http://www.roland-riegel.de/nload/nload-0.7.4.tar.gz
tar -xf nload-0.7.4.tar.gz
cd nload-0.7.4
./configure
make && make install