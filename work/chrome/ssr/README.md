前提是有ssr帐号，
如果自己配置，使用https://github.com/ywsswy/shadowsocks_install，sudo bash shadowsocks-all.sh，
使用3-Go,443端口，chacha20加密，然后就会当成服务运行在服务器上。


然后手机/win/linux日ssr连接配置好即可，一般只需server(服务器ip)，server_port:443，method:chacha20,protocal:origin，obfs:plain，其他能空则空

如果是linux，没有ssr客户端，则用命令行，参考https://samzong.me/2017/11/17/howto-use-ssr-on-linux-terminal/
config时自己设置好

privoxy要装全局模式

浏览器要在命令行启动
chromium-browser --proxy-server="http://127.0.0.1:8118"
