# 开启ssh服务/允许别人连接
- 安装好（检查是否有/etc/ssh/ssh_host_rsa_key文件）若没有，重新安装openssh-server
- windows22端口被占了，所以要被连接的话，要在/etc/ssh/sshd_config中修改port
- /etc/ssh/sshd_config 中的Port是自己作为服务方的服务端口（别人要连接的话要 -P 指定的）  /etc/ssh/ssh_config 中的Port是自己作为客户端，连接别人时默认用什么端口，如果指定了-p这里就无效了
- ssh-keygen -t rsa -C "ywsswy@qq.com" -b 4096 #生成自己的密钥对
- ssh-keygen -t ecdsa -C "ywsswy@qq.com" -b 521 # github现在要求这个(不过.ssh目录存在两种私钥的时候优先使用rsa，所以这里可以临时重命名一下rsa，或者~/.ssh/config指定使用的私钥)
- /<home>/.ssh/authorized_keys中保存了谁的公钥（且authorized_keys的权限应该是600，不是644？通过查看/var/log/secure可以定位问题），才允许谁连接
- sudo service ssh start
## 密码连接的方法

# 连接别人/从别人那接受/往别人那发送 （收发推荐rsync，其次scp）
- 用自己的私钥连接别人（对方的authorized_keys需要存了自己的公钥，-i参数是自己的私钥，自己的私钥权限应该是600）
scp -i ~/yfolder/ywsssh -r /home/hill/yfolder/proj/test_vsc ubuntu@111.230.151.212:~/        上传本地文件到服务器
ssh <user>@localhost -p 23 -i ~/.ssh/id_rsa
// 报错kex_exchange_identification: read: Connection reset by peer 一般就是因为没有指定端口

# 其他
- 如果一台机器需要通过堡垒机中转登陆，可以在secureCRT中设置Logon Actions Automate logon ogin（输入ssh完整命令）assword
- /etc/ssh里面的pub没起到作用
- 公钥.pub的格式是
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC2VW1o0Y+N9
8nOyuFLy+2H4JIi21Xt6YuUY9Or2m7Thr5nqZwFZORvQRXyff
83KDn6zFy3kxvhEq+9kzuqca2jT05zDnM38ruugBuzcSTp+OO
AHFB39t2aT/iVLJ7vK0dsPoiXR ql1119079560@yahoo.com
- 私钥的格式是
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAtlVtaNGPjfbbPw4sgJSlU22n8GnCGZ7yGTgwIagdPJzsrhS8
vth+CSIttV7emLlGPTq9pu04a+Z6mcBWTkb0EV8n33U7eiDjW/oPg834gm4hyBLJ
AR5N9M108a/TwTyFsKSGzNaXTHLD2KGKoU57WBryFhFwiZq9yfc=
-----END RSA PRIVATE KEY-----


【开机自启动-待实验

update-rc.d ssh enable  //系统自动启动SSH服务
update-rc.d ssh disabled // 关闭系统自动启动SSH服务
