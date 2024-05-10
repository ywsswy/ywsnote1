ping.exe，arp.exe,  ftp.exe, ipconfig.exe, tracert.exe,  net.exe, route.exe
ping.exe验证与远程计算机的连接
arp: 显示和修改IP地址与物理地址之间的转换表
ARP -A,查询系统中缓存的ARP表。ARP表用来维护IP地址与MAC地址的一一对应。
【ARP欺骗原理：ARP表被篡改了，使得电脑找不到通向目的地的真正的合法MAC地址，信息传达不出去或者传达错误。传达错误，有可能传达的数据被人恶意截走（被ARP伪装的计算机截走）。如果你传达的是QQ的帐号和密码，可能就会发生QQ帐号被盗的情况。
【神命令
route print
route add 202.202.32.0 mask 255.255.240.0 172.33.0.1
