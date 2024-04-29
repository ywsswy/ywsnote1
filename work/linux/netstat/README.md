```
netstat -anptu |grep <port>  # 查看端口占用
-a 显示所有状态（默认只显示connected的）要跟-t和-u等类型配合使用
-n 以数字形式显示地址和端口号，而不会尝试解析成域名格式
-p 额外显示pid和进程名（windows没有，windows用-o参数）
-t Socket=tcp（windows没有）
-u Socket=udp（windows没有）
-x Socket=unix, Active UNIX domain sockets (servers and established)（应用于同一台主机的进程间通讯-IPC）
-l 只显示listen的（比-a信息少）
```

## 输出示例
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local-Address(本机程序创建的) Foreign-Address State PID/Program name

tcp 0 0 127.0.0.1:5601 0.0.0.0:* LISTEN 3507/node  # 表明运行的是node程序（pid3507），这个程序占用5601端口（只接受本地localhost请求，公网直接请求5601端口是不行的，只能本地请求5601端口，这个具有安全保护作用），处于LISTEN状态可接受外部请求

tcp 0 0 10.3.4.63:22 132.22.4.11:2859 ESTABLISHED  # 表明有人ssh登录了这台机器，那个人的ip地址是132.22.4.11，处于ESTABLISHED  状态表示双方已经在数据交互中了


QQQQQ
3、CLOSE_WAIT
    对方主动关闭连接或者网络异常导致连接中断，这时我方的状态会变成CLOSE_WAIT 此时我方要调用close()来使得连接正确关闭
4、TIME_WAIT
    我方主动调用close()断开连接，收到对方确认后状态变为TIME_WAIT。TCP协议规定TIME_WAIT状态会一直持续2MSL(即两倍的分段最大生存期)，以此来确保旧的连接状态不会对新连接产生影响。处于TIME_WAIT状态的连接占用的资源不会被内核释放，所以作为服务器，在可能的情况下，尽量不要主动断开连接，以减少TIME_WAIT状态造成的资源浪费。
```
