DNS污染：“让”DNS服务器把对应域名解析成错乱的ip地址；
IP/关键词黑名单：丢弃访问某些ip/含某些数据的数据包；

代理服务器是个大的概念：搭建在某个地方，用来“代”我们发起某请求的服务器都可以称作是代理服务器；
通常代理服务器跟本地客户端之间都是会进行数据加密的；

## 举例
```
1）我访问 服务器中用flask中+urllib.request启动的服务时，就相当于在使用代理服务器；

2）我购买云主机，ssh登陆到云主机上然后访问（wget/curl/browser open）其他网站时，这个也相当于使用代理服务器；
2.1）更简单的：ssh动态端口转发+Chrome/edge+SwitchyOmega扩展(配置socks5代理localhost:<localport>)
手机端用Termux进行ssh，用68.xx版本的firefox浏览器[about:config配置socks5代理](https://www.bannedbook.org/bnews/fanqiang/20160205/499416.html)

3）VPN（Virtual Private Network/虚拟专用网络）：
上两种的服务器还具备其他功能，如果单纯就为了代理的话，可以只搭建VPN服务器；
本地/用户需要安装一个VPN客户端（会创建一个虚拟网卡），所有数据是通过虚拟网卡跟VPN服务器进行通信的；
PPTP(点对点隧道协议)、L2TP(层2隧道协议)、SSTP(安全套接字隧道协议)、IPSec(IP安全协议)、OpenVPN、IKEv2和GRE(通用路由封装)隧道都是VPN技术的不同实现方式；

4）ShadowSocks（SS）
是一个协议，使用这种协议进行通信的话，
本地需要安装一个SS客户端（客户端里可以配置不同的加密协议），服务端是VPS/机场；
相比VPN，流量特征没那么容易被识别；

5）shadowsocksR类似于SS+plugin，
区别在于伪装消除流量特征的方案更优秀，比如可以通过添加plugin，伪装成http协议；

6）trojan也是一个协议
伪装成了https协议，所以搭建需要证书

7）vmess也是一个协议

```
ps）
- V2ray是一个网络工具，它原创了并支持vmess协议，同时也支持ss和trojan
- clash也是一个网络工具
- VPS与机场的区别：
VPS（virtual private server）是一个节点（可自己搭建）；
机场是一群节点（通常是运营商提供，用户来购买，使用时可以选择机场中较好的节点，非常灵活（地域、稳定、简单不用自己维护））；
- 查看某地网络访问某目标ip时的路由线路：
https://tools.ipip.net/traceroute.php


经验：如果代理服务器的出入带宽能到1.5Mbps左右，那么视频卡顿就跟代理服务器没关系了，考虑是否是客户端或者源站或者运营商或者信号的问题；而且不同时间的网络差异可能很大，总之境外的服务器非常不靠谱，不要把可能提供资源下载的服务器架设在国外；