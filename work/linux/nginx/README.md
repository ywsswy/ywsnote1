[1] yum安装
[2] 源码安装
http://nginx.org/en/download.html
tar -xzf nginx-1.16.1.tar.gz
cd nginx-1.16.1

# 有些可能需要 autogen.sh 或者 autoreconf -v --install
# 别忘了，可以./configure --help
./configure --prefix=/home/<usr>/software/nginx #仅当前用户安装则需要指定prefix
            --enable-debug #不会加-O2优化
make && make install

修改安装目录中的conf/nginx.conf

启动sbin/nginx(哪怕是yum安装的也是这么启动即可，根本不需要指定service或者daemon)，可以-c指定配置文件
重启nginx -s reload

## 其他
- 其实nginx和flask的区别，前者适合静态&目录映射&简单鉴权，后者可以处理逻辑
- 500错误，可以查看nginx日志，/var/nginx/error.log
- nginx -t  # 可以测试配置文件是否存在问题
