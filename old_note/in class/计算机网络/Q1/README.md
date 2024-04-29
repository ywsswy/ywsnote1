1．ping.exe验证与远程计算机的连接。该命令只有在安装了 TCP/IP 协议后才可以使用。
ping [-t] [-a] [-n count] [-l length] [-f] [-i ttl] [-v tos] [-r count] [-s count] [[-j computer-list] | [-k computer-list]] [-w timeout] destination-list
参数：
-t  ping 指定的计算机直到中断。
-a 将地址解析为计算机名。
//ping -a hongyan.cqupt.edu.cn
-n count 发送 count 指定的 ECHO 数据包数。默认值为 4。
//ping -n 5 202.202.43.125
-l length 发送包含由 length 指定的数据量的 ECHO 数据包。默认为 32 字节；最大值是 65,527。
//ping -l 555 202.202.43.125
-f 在数据包中发送“不要分段”标志。数据包就不会被路由上的网关分段。
-i ttl将“生存时间”字段设置为 ttl 指定的值。
-v tos 将“服务类型”字段设置为 tos 指定的值。
-r count 在“记录路由”字段中记录传出和返回数据包的路由。count 可以指定最少 1 台，最多 9 台计算机。
-s count 指定 count 指定的跃点数的时间戳。
-j computer-list 利用 computer-list 指定的计算机列表路由数据包。连续计算机可以被中间网关分隔（路由稀疏源）IP 允许的最大数量为 9。
-k computer-list 利用 computer-list 指定的计算机列表路由数据包。连续计算机不能被中间网关分隔（路由严格源）IP 允许的最大数量为 9。
-w timeout 指定超时间隔，单位为毫秒。
destination-list 指定要 ping 的远程计算机。
较一般的用法是 ping -t bbs.cqupt.edu.cn

3.arp 查看与本机有过来往的地址 显示和修改IP地址与物理地址之间的转换表
ARP -s inet_addr eth_addr [if_addr]
ARP -d inet_addr [if_addr]
ARP -a [inet_addr] [-N if_addr]
  -a            显示当前的ARP信息，可以指定网络地址
  -g            跟 -a一样.
  -d            删除由inet_addr指定的主机.可以使用* 来删除所有主机.
  -s            添加主机，并将网络地址跟物理地址相对应，这一项是永久生效的。
  eth_addr      物理地址.
  if_addr       If present, this specifies the Internet address of the
interface whose address translation table should be modified.If not present, the first applicable interface will be used.
4. ftp：（功能就不用描述了，请参看下面的具体用法）
该命令只有在安装了 TCP/IP 协议之后才可用。Ftp 是一种服务，一旦启动，将创建在其中可以使用 ftp 命令的子环境，通过键入 quit 子命令可以从子环境返回到 Windows 2000 命令提示符。当 ftp 子环境运行时，它由 ftp 命令提示符代表。
ftp [-v] [-n] [-i] [-d] [-g] [-s:filename] [-a] [-w:windowsize] [computer]
参数：
-v 禁止显示远程服务器响应。
-n 禁止自动登录到初始连接。
-I  多个文件传送时关闭交互提示。
-d 启用调试、显示在客户端和服务器之间传递的所有 ftp 命令。
//ftp -d 172.16.38.100
-g 禁用文件名组，它允许在本地文件和路径名中使用通配符字符（* 和 ?）。（请参阅联机“命令参考”中的 glob 命令。）
-s: filename指定包含 ftp 命令的文本文件；当 ftp 启动后，这些命令将自动运行。该参数中不允许有空格。使用该开关而不是重定向 (>)。
-a 在捆绑数据连接时使用任何本地接口。
-w:windowsize 替代默认大小为 4096 的传送缓冲区。
Computer 指定要连接到远程计算机的计算机名或 IP 地址。如果指定，计算机必须是行的最后一个参数。
下面是一些常用命令：
！： 从ftp子系统退出到系统外壳
？：显示ftp说明，跟help一样
append: 添加文件，格式为：append 本地文件 远程文件
cd： 更换远程目录
lcd： 更换本地目录，若无参数，将显示当前目录
open：与指定的ftp服务器连接 open computer [port]
close：结束与远程服务器的 FTP 会话并返回命令解释程序
bye：结束与远程计算机的 FTP 会话并退出 ftp
dir： 结束与远程计算机的 FTP 会话并退出 ftp
get 和 recv：使用当前文件转换类型将远程文件复制到本地计算机 get remote-file [local-file]
send 和 put：上传文件：send local-file [remote-file]
其它命令请参考帮助文件。

