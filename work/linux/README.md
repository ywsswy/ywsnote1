[funny software]
select-editor #选择操作系统默认编辑器（crontab和w3m等会使用）
wdiff -t a b |less -R #删掉的是下划线，新增的是反选
/etc/redhat-release #操作系统版本
sudo yum install bash-completion # tab补全
astyle -k3 --style=bsd <file> #源代码格式化
usleep 1000000
watch -d -n 1 w
timeout 3 top #设置一个命令的超时时间，超时返回码124， [返回码](https://blog.csdn.net/nicai_xiaoqinxi/article/details/85055086)
tree <folder> -I '<pattern>' #排除某些文件夹进行打印目录树
netstat -tl
netstat -pan |grep <port> #查看端口占用
lsof -i:<port>
fc-list #查看已安装字体及路径，rm即可删除
U盘文件损坏，先查看/var/log/syslog确诊 然后sudo umount /media/hill/AC 最后sudo dosfsck -v -a /dev/sdb1修复
convert src.png -crop 100x80+60+40 desc.png  #使用imagemagick 剪切图片区域（宽x高+x+y）
route add -host 202.202.32.202 gw 172.18.112.1 #让教务在线走有线网卡
sudo route del -net default gw 172.18.112.1 #删除内网的那条默认路由
ss -nl 显示tcp连接端口等 
xrand -s 800x480 改分辨率
sudo poweroff 关机
echo "scale=7; 1 / 2" |bc #bc计算小数除法的时候，必须指定精度

ctrl+alt+f1~6进入真正终端tty1~tty6，ctrl+alt+f7返回图形窗口;exit退出终端
\+Enter可以连接两行

PRO：
env
从环境变量中删除变量