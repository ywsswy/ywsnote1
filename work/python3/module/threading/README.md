time.sleep(1.5) #1.5:unit(s)   
#这是阻塞本线程的延时（本，意思就是子线程的sleep影响不到主线程，反之亦然）

threading.Timer(inter, jwzx_monitor).start() # inter:unit(s),jwzx_monitor:<function_name>
#这是不阻塞本线程（开启了新线程）的延时，此刻创建线程，等待inter秒后执行<function_name>函数

如果jwzx_monitor函数需要参数，则在后面加个tuple参数放入

值得一提的是由于GIL锁，多线程仍然无法使cpu使用率超过100%
//////////////////////////////////////////////////////////////////////

https://www.cnblogs.com/tkqasn/p/5700281.html
【模块方法
threading.currentThread(): 返回当前的线程变量 
threading.enumerate(): 返回正在运行(start了且没有死）的线程的list
threading.activeCount(): 返回正在运行的线程数量，等价于len(threading.enumerate())
threading.Timer(<seconds>, <func_name>)：构造Timer(threading的子类），启动后延时s秒后执行f函数
【进程变量/实例方法
start() #启动线程
set/getName() #获取设置线程名字
is/setDaemon(bool): 获取/设置守护线程（线程默认都是非守护线程（False）），set必须在start前
主线程会等待所有【非守护线程】的子线程全结束后才能结束，即使主线程语句都结束了
主线程结束，所有的子线程都会结束

要想随时全部相应Ctrl+C死亡，就设置成Daemon


子线程有用户输入

各线程print互不冲突，都是标准输出
各线程input等待输入是有先来后到的

