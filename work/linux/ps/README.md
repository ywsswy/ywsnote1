ps axfu 既能看到关系树，又能看到启动时间
ps -eo pid,lstart,etime,cmd |grep 这里的lstart是详细启动时间

D 不可中断的休眠。通常是IO。
R 运行。正在运行或者在运行队列中等待。
S 休眠。在等待某个事件，信号。
T 停止。进程接收到信息SIGSTOP，SIGSTP，SIGTIN，SIGTOU信号。
W paging，在2.6之后不用。
X 死掉的进程，不应该出现。
Z 僵死进程。
## 通常还会跟随如下字母表示更详细的状态。
< 高优先级
N 低优先级
L 有pages在内存中locked。用于实时或者自定义IO。
s 进程领导者，其有子进程。
l 多线程
+ 位于前台进程组。