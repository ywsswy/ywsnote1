```
user nginx;  # 启动的worker process的用户，如果不使用autoindex目录浏览文件之类的话没问题，否则可能没有权限去看其他目录文件，不过这个功能有风险，建议临时打开目录的话用python3 -m http.server <port>，非要用的话，要么就改成root，要么新建个临时的nginx用户和目录
http {
  client_max_body_size 1000m;  # 最大传输大小（如果设置小，那么上传大文件会失败）
  server {
    listen 80;
    server_name  _;  # 如果就一个server，那这个可以随便填，如果多个就需要改以免匹配错
    location /test {
      gzip on;  # 是否对某些资源开启gzip
      gzip_comp_level 5;  # 压缩级别，9最高，但是浪费CPU，5的压缩比就足够了
      gzip_types application/octet-stream;  # 对哪些资源开启gzip，即响应头中的Content-Type，服务端nginx负责压缩，客户端浏览器检测到Content-Encoding为gzip之后会进行解压
      auth_basic "Please input password";  # 可选，配合auth_basic_user_file
      auth_basic_user_file /path/to/passwd;  # 可选，表示需要输入密码验证，通过htpasswd -c <file> <user_name> 命令生成baseAuth
      autoindex on; 可选，配合root
      root /path/to/root/file ;  # 可选，映射机器本地根目录，例如请求xxx:80/test/index.html，相当于请求本机的/path/to/root/file/test/index.html文件
      proxy_pass http://127.0.0.1:65432;  # 可选，表示反向代理（不是映射到目录而是把请求转发），请求xxx:80/test时，相当于请求 http://127.0.0.1:65432/test
    }
  }
  server {
    listen 443 ssl;
    server_name  zzz.com;
    ssl_certificate zzz.com_bundle.crt;  # ssl证书路径
    ssl_certificate_key zzz.com.key;  # ssl证书路径
    ssl_session_timeout 5m; 
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; 
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
    ssl_prefer_server_ciphers on; 
    location ... # 同上
  }
}
```