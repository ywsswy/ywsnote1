socket的名词含义有很多：
1）在计算机网络原理中表示的tcp连接的端点/套接字；
2）socket接口：操作系统内核提供的API，是传输层和应用层之间的“桥梁”（不同操作系统有不同的实现，例如有winsock）；
3）某些lib库/函数名；


套接字在客户端和服务端的流程：
1）通常服务端固定一个端口监听请求（调用socket+bind+listen创建一个用于监听的socket-1）；
2）服务端（内核）收到客户端的连接请求时就会“新”创建一个socket-2（用于互相read & write收发数据），原来的socket-1只用于监听；
3）客户端调用socket & connect创建socket-3并请求连接服务端，实际连接的是socket-2；
4）服务端（用户程序）可以不断调用accept（如果没有连接会阻塞）会逐个取出建立好的socket-2，socket-4……；（取出来的netstat -p能看到pid，没取出来的还在内核中看不到pid）

套接字编程：
https://blog.csdn.net/qq18218628646/article/details/131821847