win7 VPN连接（PPTP）：
http://jingyan.baidu.com/article/ce09321b23cf7c2bff858fb8.html
打开网络和共享中心
设置新的连接或网络
连接到工作区
使用我的Internet连接VPN
Internet地址：172.21.15.247
用户名 密码
【不安全？
http://www.2cto.com/article/201306/223751.html
live http replay
smartsniff的神软可以对虚拟网卡进行抓包。从抓到的数据包很筛选出http的包
windows自带的远程和路由访问，里面状态发现一个不明来客的访问。查看发现dhcp分配给他的ip是10.250.1.7，使用arp -a也无法得到该人的mac地址，然后使用windows的cmd命令行执行netstat -an|find “1723″ ，ok，发现了此不速之客的宿舍内网IP地址10.xx.6.65
C:\Documents and Settings\IUSR_XXXX>netstat -an|find “1723″
TCP    0.0.0.0:1723           0.0.0.0:0              LISTENING
TCP    202.xxx.xx.xx:1723    10.xx.6.65:53401       ESTABLISHED
http://tieba.baidu.com/p/2715197276
