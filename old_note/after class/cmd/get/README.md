//////////////////////////////////////
cmd	进入命令太
mstsc	远程桌面登录
%systemroot%系统根目录，通常为C:windows
%USERPROFILE%通常为C:users\admin
环境变量，path里的目录对计算机来说，相当于当前目录，便于查找
Pathext,相当于
nul		nul表示空设备，从概念上讲，它不可见，
		存在于每个目录中，可以把它看成一个特殊的“文件”，
		它没有内容；一般可把输出写入到nul，来达到屏蔽输出的目的，
		如pause>nul，此命令执行效果是暂停，并且不会显示“请按任意键继续. . .”
cd../../	在当前目录到上两级目录
cd.>a.txt	cd.表示改变当前目录为当前目录，即等于没改变；而且此命令不会有输出。
		>表示把命令输出写入到文件。后面跟着a.txt，就表示写入到a.txt。
		而此例中命令不会有输出，所以就创建了没有内容的空文件。
cd admin	进入当前文件夹下的amdin文件夹
cdd		打开当前文件夹下的cdd文件
cls		清输出屏
COLOR    	设置默认控制台前景和背景颜色。
COPY/?		(找COPY下接参数的功能)
		copy nul a.txt
mode con cols=63 （最大170宽）
mode con  lines=32（最大44高）
mode con cols=63 lines=32
d:		读D盘
dir
	dir /a:h	显示当前目录下的隐藏文件

Del 		删除该文件夹里的直系文件或删除当前文件（不删除文件夹，以及间接文件）
dir打		开目录
Exit 		退出cmd
fsutil file createnew 1.txt 1024 # 创建1024个字节的文件
Help　		打开Dos命令
md admin	在此处建立admin文件夹
path 		列出系统环境变量
Rd		删除空文件夹
ver		查看版本
start <shortcut.lnk name>	启动快捷方式
tab		键自动补全
type nul>a.txt	此例子表示显示空设备的内容，并写入到a.txt。

