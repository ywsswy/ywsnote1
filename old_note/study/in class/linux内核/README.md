守护进程（daemon）
如果一个进程永远都是以后台方式启动，并且不能受到Shell退出影响而退出，一个正统的做法是将其创建为守护进程（daemon）。守护进程值得是系统长期运行的后台进程，类似Windows服务。
ps ajx|less
其中TPGID一栏为-1就是守护进程
tail /tmp/watch_stdout.log  -f
1）fork一个子进程
2）setsid：脱离控制
3）第二次fork：保证其再生成的子进程就不是领导进程，进而防止其打开终端
4）工作目录
5）unmask：恢复掩码
6）关闭描述符，防止资源占用
守护进程需要
设置文件模式创建屏蔽字。
没有控制终端。
确定工作目录。
关闭不再需要的文件描述符。
告别标准输入输出
守护进程在后台默默运行不受控制终端的控制。这里是通过setsid() 函数来实现。调用这个函数的效果是：
1）创建一个新会话（session） 
2）创建一个新进程组。 
3）调用进程成为 新会话 的首进程 
4）调用进程成为 新进程组 的组长进程 
5）调用进程失去控制终端
打印进程
孤儿进程
umask值002 所对应的文件和目录创建缺省权限分别为6 6 4(666 减 2)和7 7 5(777 减 2)。
或者日志格式是-PID 或者是获取不到PID则-1
http://blog.csdn.net/asd7486/article/details/51966225
http://blog.csdn.net/qq_35409955/article/details/71599289
http://blog.csdn.net/xiyoulinux_kangyijie/article/details/72716682

