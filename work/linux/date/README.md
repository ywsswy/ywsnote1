date +"%Y-%m-%d %H:%M:%S.%N" 
date -d @1572838648 

date +%s 当前时间戳（秒级）
date +%s -d "2019-11-27 19:21:47" #某个时刻的时间戳

写在脚本里不能直接运行上面的命令
而是要用 eval ${cmd}，引号转义

date -d yesterday # 

# 设置本地时间，重启会失效（crontab使用的是本地时间）
sudo date -s "2019-06-25 20:13:00"

# 查看系统时间，重启也能保留
sudo hwclock --show

# 把系统时间同步到本地时间
sudo hwclock --hctosys

# 把本地时间同步到系统时间
sudo hwclock --systohc --localtime

# 更新上海的互联网系统时间，可能导致很多程序不可用哦，谨慎操纵，多操作几次，既能更新
sudo ntpdate -u ntp.api.bz