【1. 关于某个文件名的『类型』侦测(存在与否)，如 test -e filename  
-e 该『文件名』是否存在？(常用)  
-f 该『文件名』是否为文件(file)？(常用)  
-d 该『文件名』是否为目录(directory)？(常用)  
-b 该『文件名』是否为一个 block device 装置？  
-c 该『文件名』是否为一个 character device 装置？  
-S 该『文件名』是否为一个 Socket 文件？  
-p 该『文件名』是否为一个 FIFO (pipe) 文件？  
-L 该『文件名』是否为一个连结档？ 
【2.流程控制&&shell脚本基础语法
#!/bin/sh
file="a.out"
if [ ! -f "$file" ];then
echo "文件不存在"
fi
#注意空格必不可少，因为这是命令/kb
#if [ ! -f "str" ]这些都要前后有空格，但是;是结尾，可以当作空格？起始可以直接起始？
#变量定义的时候不写$,使用的时候要写$
if list ;then
	do something here
elif list ;then
	do another thing here
else
	do something else here
fi
【3.Linux文件类型
1）普通文件类型 
Linux中最多的一种文件类型, 包括 纯文本文件(ASCII)；二进制文件(binary)；数据格式的文件(data);各种压缩文件.第一个属性为 [-] 。
2）目录文件
就是目录， 能用 # cd 命令进入的。第一个属性为 [d]，例如 [drwxrwxrwx]。
3）字符设备或块设备文件
块设备文件  ： 就是存储数据以供系统存取的接口设备，简单而言就是硬盘。例如一号硬盘的代码是 /dev/hda1等文件。第一个属性为 [b]。
字符设备文件：即串行端口的接口设备，例如键盘、鼠标等等。第一个属性为 [c]。
4）套接字文件
这类文件通常用在网络数据连接。可以启动一个程序来监听客户端的要求，客户端就可以通过套接字来进行数据通信。第一个属性为 [s]，最常在 /var/run目录中看到这种文件类型。
5）管道文件
FIFO也是一种特殊的文件类型，它主要的目的是，解决多个程序同时存取一个文件所造成的错误。FIFO是first-in-first-out(先进先出)的缩写。第一个属性为 [p]。
6）链接文件
类似Windows下面的快捷方式。第一个属性为 [l]，例如 [lrwxrwxrwx]。
【4.mknod 命令
mknod 设备文件名[/dev/xyz]  b/c  主号  次号
{  mkdir /dev/vg01
   mknod /dev/vg01/group  c  64  0X010000
}
创建之后，就可以使用你想要创建的设备对于德创建命令了
3）
守护进程
-c ：建立一个压缩文件的参数指令(create 的意思)；
-v ：压缩的过程中显示文件！
-f ：使用档名
tar -zcvf /tmp/etc.tar.gz /etc   #打包后，以 zip压缩
运行的时候在后面加上“&”符号，目的是让这个脚本脱离终端运行：
bash bash-deamon.sh &
kill -9 [pid]结束进程
4）
参数判断
文件存在
获取正确的 主设备号 从设备号 （去dev下查看）
5)
【添加
useradd    注：添加用户
groupadd
【查看
/etc/group文件包含所有组
/etc/passwd系统存在的所有用户名
/etc/group |grep class查看组内成员
【删除
userdel peter
groupdel class1
【入组
usermod -G class1 gar1
-le 小于等于
循环
myvar=1
while [ $myvar -le 10 ]
do
        echo $myvar
        myvar=$(( $myvar + 1 ))
done

