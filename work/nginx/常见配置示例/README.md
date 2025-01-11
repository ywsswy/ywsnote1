```
user nginx;  # 启动的worker process的用户，用户权限影响autoindex访问权限
http {
  client_max_body_size 1000m;  # 最大传输大小（如果设置小，那么上传大文件会失败）
  server {
    listen 80;
    server_name  _;  # 如果就一个server，那这个可以随便填，如果多个就需要改以免匹配错
    location /test {
      gzip off;  # 是否对某些资源开启gzip，开启不一定会加速，仅在网络环境非常好但文件很大的情况下考虑开，跨境不开
      gzip_comp_level 5;  # 压缩级别，9最高，但是浪费CPU，5的压缩比就足够了
      gzip_types application/octet-stream;  # 对哪些资源开启gzip，即响应头中的Content-Type，服务端nginx负责压缩，客户端浏览器检测到Content-Encoding为gzip之后会进行解压
      gzip_vary on;
      auth_basic "Please input password";  # 可选，配合auth_basic_user_file
      auth_basic_user_file /path/to/passwd;  # 可选，表示需要输入密码验证，开启的话登陆后每次浏览器的请求头都会有Authorization: Basic base64(<user>:<pwd>)；通过htpasswd -c <file> <user_name> 命令生成baseAuth；如果是windows系统，路径虽然是backslash（"\"）的，但是这里也可以写成forward slash的
      autoindex off; 可选，配合root，不过这个功能可能不小心暴露敏感文件，建议关闭；分享文件可以“使用网盘”；退一万步来说，就算需要临时打开目录的话更简单的方法是python3 -m http.server <port>；
      root /path/to/root/file ;  # 可选，映射机器本地根目录，例如请求xxx:80/test/index.html，相当于请求本机的/path/to/root/file/test/index.html文件
      proxy_pass http://127.0.0.1:65432;  # 可选，表示反向代理（不是映射到目录而是把请求转发），请求xxx:80/test时，相当于请求 http://127.0.0.1:65432/test
    }
  }
  server {
    listen 443 ssl;
    server_name  zzz.com;
    ssl_certificate zzz.com_bundle.crt;  # ssl证书(使用的nginx证书类型，而不是apache？)验证证书是否安装成功，可以在客户端执行openssl s_client -connect zzz.com:443 -servername zzz.com | openssl x509 -noout -dates，而不是通过import ssl print(ssl.get_default_verify_paths())来验证
    ssl_certificate_key zzz.com.key;
    ssl_session_timeout 5m; 
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; 
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
    ssl_prefer_server_ciphers on; 
    location ... # 同上
  }
}
```


### 反向代理是在靠近服务端侧搭建的代理
（1）客户端什么都不需要配置，客户端浏览器地址栏输入的就是代理服务器的地址（客户端根本不知道有没有代理，不知道实际提供服务的服务端，可能以为服务端就是这个地址）；
（2）代理服务器如果使用nginx实现，配置文件格式如上；
（3）代理服务器收到请求后，内部转发到真实的服务器上去；

### 正向代理是在靠近客户端侧搭建的代理
（1）客户端需要配置代理服务器的地址，虽然浏览器地址栏还是输入真实服务器的url地址，但是其实这个请求是发送到代理服务器上，由它帮忙请求；
（2）代理服务器如果使用nginx实现，配置文件格式形如（其中的http_host和request_uri每次请求会由nginx帮忙自动填充好的）：`proxy_pass http://$http_host$request_uri;`，需要注意的是官方的正向代理是不支持代理https请求的（不过由一个大佬拓展了一个ngx_http_proxy_connect_module模块支持了）；
（3）服务端没有任何操作，服务端不知道实际发起请求的客户端ip是多少；

