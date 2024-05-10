可以用于启动一个tcp连接的监听（telnet 可以发起一个tcp的连接，所以二者可以作为客户端和服务端建立连接互相收发消息）

nc -lv localhost 9999

能检测tcp & udp 端口并且是区间
nc -vz <ip> 134-144