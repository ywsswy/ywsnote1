工作原理：基于winpcap，把网卡设置为混杂模式（能让网卡接收（不丢弃）所有的流过网卡的帧），然后它的驱动程序能将数据不加处理地（避免TCP/IP协议的驱动剥离）将给用户程序进而分析；



显示过滤器

过滤器写法
((ip.src == 192.168.0.1) and (ip.dst == 192.168.0.2)) or ((ip.src == 192.168.0.2) and (ip.dst == 192.168.0.1))
(ip.dst eq 210.47.0.14) && (http)
tcp.dstport
tcp.srcport


## 软件使用
清除当前捕获的数据/清屏重新开始：绿色按钮

## 其他
抓python urllib的包是不同的，需要reassembled后才能恢复

抓https需要wireshark版本2，

通常在目的机器写来源ip.src靠谱一些，写ip.dst有可能经过转换了