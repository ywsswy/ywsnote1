http://nginx.org/en/download.html
tar -xzf nginx-1.16.1.tar.gz
cd nginx-1.16.1

# 有些可能需要 autogen.sh 或者 autoreconf -v --install
# 别忘了，可以./configure --help
./configure --prefix=/home/<usr>/software/nginx #仅当前用户安装则需要指定prefix
            --enable-debug #不会加-O2优化
make && make install

修改安装目录中的conf/nginx.conf
http.server.listen #端口号
http.server.location # 这部分路径用户需要在url中匹配上，root加上这个等于真实的服务器物理路径，这里最好不用/结尾，对用户友好些
http.server.location.root # 这部分路径对用户来说透明
http.server.location.autoindex on; #则表示用户可以访问目录

启动sbin/nginx(哪怕是yum安装的也是这么启动即可，根本不需要指定service或者daemon)
重启nginx -s reload

## example:
    server {
        listen       520;
        server_name  localhost;
        location /c/d {
            root   ../../a/b/;
            autoindex on; 
            # proxy_pass http://baidu.com; 如果不是访问本地，而是其他服务
        }

## 其实nginx能做到的flask也能做，只需要在static/file目录下ln -s即可，所以仅当必须给用户查看目录的情况下才会用到nginx

## 其他
- 500错误，可以查看nginx日志，/var/nginx/error.log
- htpasswd -c <file> <user_name> # 生成一个baseAuth来给nginx使用
```
    server {
      listen xxxx;
      server_name  localhost;
      auth_basic "Please input password";
      auth_basic_user_file /etc/nginx/yws_passwd;
      location / { 
        proxy_pass http://127.0.0.1:yyyy/;
      }   
    }
```
- nginx -t  # 可以测试配置文件是否存在问题