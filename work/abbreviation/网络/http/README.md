HTTP/1.1会默认开启长连接（相当于协议头写了Connection: keep-alive）

http客户端（浏览器）和服务端会遵守实现约定，不会断开连接（close套接字）；
主动断开连接的一方（通常是服务端）会在协议头写Connection:Close，并且close套接字；