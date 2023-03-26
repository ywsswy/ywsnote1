          minute hour  day-of-month month-of-year day-of-week      commands 
    合法值 00-59  00-23 01-31        01-12         0-6(0 is sunday) commands（代表要执行的脚本）
    除了数字还有几个个特殊的符号就是"*"、"/"和"-"、","，*代表所有的取值范围内的数字，"/"代表每的意思,"/5"表示每5个单位，"-"代表从某个数字到某个数字,","分开几个离散的数字。

eg
#每晚的21:30重启apache。
30 21 * * * /usr/local/etc/rc.d/lighttpd restart

#每两个小时
0 */2 * * * date

# 启动目录是home目录

# 没有考虑是否重入交叉执行