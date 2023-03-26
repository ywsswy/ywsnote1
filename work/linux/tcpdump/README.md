tcpdump -i <网卡名称> tcp and port <端口号> -n -nn -v -vv -w cache -W 100:100个文件 -C 30:文件大小30M -s 100：只抓包前面的100字节  -Z <用户名称如root>

示例:
tcpdump -i eth0 tcp and port 6379 -n -nn -v -vv -w cache -W 50 -C 100 -s 100 -Z root

-w是自动写文件
生成的文件可以用
tcpdump -r <file> 来读取，展示格式是：
```
20:29:45.284827 IP 9-250-205-209.12691 > 183.158.84.54.9273: Flags [P.], seq 1:396, ack 2542, win 35, length 395
20:29:45.290063 IP 9-250-205-209.60510 > 15.36.52.172.9273: Flags [P.], seq 504:560, ack 33731, win 173, options [nop,nop,TS val 4168217983 ecr 2756072661], length 56
```
<时间：20:29:45.282975> <协议：IP> <源地址ip.port> > <目的地址ip.port>: <其他>
有时候需要关注的有win（源告诉目的可以接收多大的回包），如果win太小，那么回完全部的包可能就非常慢。