[funny software]
select-editor #选择操作系统默认编辑器（crontab和w3m等会使用）
wdiff -t a b |less -R #删掉的是下划线，新增的是反选
/etc/redhat-release #操作系统版本
sudo yum install bash-completion # tab补全
sleep 1 # sleep 0.5
nc -vzn <ip> <port> # 查看端口存活情况 
watch -d -n 1 w
timeout 3 top #设置一个命令的超时时间，超时返回码124， [返回码](https://blog.csdn.net/nicai_xiaoqinxi/article/details/85055086)
tree -Naf <folder> -I '<pattern>' # -I 排除某些文件夹进行打印目录树；N是显示中文，a显示隐藏，f显示全路径
fc-list #查看已安装字体及路径，rm即可删除
convert src.png -crop 100x80+60+40 desc.png  #使用imagemagick 剪切图片区域（宽x高+x+y）
route add -host 202.202.32.202 gw 172.18.112.1 #让教务在线走有线网卡
sudo route del -net default gw 172.18.112.1 #删除内网的那条默认路由
ss -nl 显示tcp连接端口等 
xrand -s 800x480 改分辨率
sudo poweroff 关机
echo "scale=7; 1 / 2" |bc -l #bc计算小数除法的时候，必须指定精度，算对数中的指数不支持小数，a^b 要写成 e(b*l(a))，如果要计算整数除法求余，就要把scale设置为0
dd if=/dev/zero of=test bs=1M count=1024 # 创建一个G的文件
rename log data *  # 把所有文件名中的log替换成data

^old^new  # 把上一条命令中的某字符串替换掉
ctrl+alt+f1~6进入真正终端tty1~tty6，ctrl+alt+f7返回图形窗口;exit退出终端
\+Enter可以连接两行

PRO：
env
从环境变量中删除变量
