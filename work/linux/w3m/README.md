https://wiki.ubuntu.org.cn/W3m

先装gc依赖 wget http://www.hboehm.info/gc/gc_source/gc-7.0.tar.gz

export LDFLAGS="-L /home/yanwenshan/software/gc/lib/"

wget http://nchc.dl.sourceforge.net/project/w3m/w3m/w3m-0.5.3/w3m-0.5.3.tar.gz

./configure --prefix='/home/yanwenshan/software/w3m' --with-gc='/home/yanwenshan/software/gc'

export LD_LIBRARY_PATH="/home/yanwenshan/software/gc/lib/"

make 
make install

H 帮助
