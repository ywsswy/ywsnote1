netstat -tl

netstat -pan |grep <port> #查看端口占用 (netstat -an |grep <port>  # macOS)
输出显示的是Local Address,Foreign Address,State,PID/Program name
例如（127.0.0.1:5601 0.0.0.0:* LISTEN 3507/node）
表明运行的是node程序（pid3507），这个程序占用5601端口，只接受本地localhost请求，公网直接请求5601端口是不行的，只能本地请求5601端口，这个具有安全保护作用


# ps
还有一个命令：
lsof -i :<port>

# macOS
lsof -i -P -n | grep LISTEN