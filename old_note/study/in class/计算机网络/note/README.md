上传12次实验日志202.202.43.106:8086
1次汇总的实验报告 ftp://172.23.2.8    bg目录下，文件名：201521.doc
一个交换机就一个网关，一个vty
但可有多个vlan，每个可有不同ip
【】
switch互连只能有一个server吗？
【】
show run
interface Vlan1
 ip address 192.168.0.8 255.255.255.0
!
interface Vlan2
 mac-address 000a.4132.8601
 ip address 192.168.0.15 255.255.255.0
!
ip default-gateway 192.168.0.254
!
!为什么vlan1 没有mac-address
【】
vlan1配置的ip为交换机的管理性ip，如果vlan2也配置了ip呢？
【】路由器两个端口不能在同一个网段；交换机则任意都可以
172.23.2.8 s 网络协议分析 
ftp://s@172.23.2.8/	这种写法直接就能登陆
ethereal抓包工具
