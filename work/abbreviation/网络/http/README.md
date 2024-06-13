HTTP/1.1会默认开启长连接（相当于协议头写了Connection: keep-alive）

http客户端（浏览器）和服务端会遵守实现约定，不会断开连接（close套接字）；
主动断开连接的一方（flask服务端默认不是长连接）会在协议头写Connection:Close，并且close套接字；

nginx默认被调是长连接？形如：
客户端 <--长连接-->  nginx <--短链接--> 服务