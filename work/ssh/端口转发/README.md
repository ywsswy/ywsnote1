下述中的内网指没有公网ip的局域网，外网指有公网ip的网络；

## 远程端口转发
```
ssh -R <ori_port>:<new_host>:<new_port> <ssh_server_user>@<ssh_server_host>
能力：跟ssh服务器建立安全通道，所有请求ssh服务器<ori_port>的请求，都会通过通道交给ssh客户端，由它转发到<new_host>:<new_port>上去；
场景：
- 背景）外网想访问内网的网站<new_host>:<new_port>；
- 1）外网搭建一个服务器<ssh_server_host>；
- 2）内网ssh客户端运行远程端口转发命令；
- 3）至此，访问<ssh_server_host>:<ori_port>，就可以达到访问<new_host>:<new_port>的效果；

```

## 动态端口转发
```
ssh -D [<ori_host>:]<ori_port>  <ssh_server_user>@<ssh_server_host>
ps：<ori_host>默认是localhost，表示必须本机程序发起的请求，如果是0.0.0.0，则支持其它主机请求过来的；
能力：跟ssh服务器建立安全通道，本机启动socks5代理，所有通过代理端口（本机:<ori_port>）发起的请求，都会通过通道交给ssh服务端，由它发起请求；之所以叫动态是因为不论请求任何目的地全部都会被代理转发，这是相对本地端口转发而差异的；
场景：
- 背景）本机访问外网的全部网站网络不通或者线路不好，但是用另一台主机<ssh_server_host>访问没问题；
- 1）本机ssh客户端运行动态端口转发命令；
- 2）配置客户端（例如浏览器、或者系统全局）sock5代理到本机:<ori_port>；
- 3）至此，本机即可访问外网的全部网站；
```

## 本地端口转发
```
ssh -L [<ori_host>:]<ori_port>:<new_host>:<new_port> <ssh_server_user>@<ssh_server_host>
能力：跟ssh服务器建立安全通道，所有请求本机:<ori_port>的请求，都会通过通道交给ssh服务端，由它转发到<new_host>:<new_port>上去；
场景：
- 背景）本机访问某外网的网站<new_host>:<new_port>网络不通或者线路不好，但是用另一台主机<ssh_server_host>访问没问题；
- 1）本机ssh客户端运行本地端口转发命令；
- 2）至此，访问本机:<ori_port>，就可以达到访问<new_host>:<new_port>的效果；
- 其他）可以跟其他命令搭配使用，例如A直接通过服务器B进行动态端口转发可能网络不好，那么可以让A先通过服务器C进行本地端口转发给B的22端口（A->C建立安全通道 ssh -L 666:<B>:22 <user_c>@<C>），然后再通过本地端口进行动态端口转发（A->C->B建立安全通道 ssh -p 666 -D 999 <user_b>@localhost）；
```

### 其他
- 端口扫描工具 https://tool.chinaz.com/port