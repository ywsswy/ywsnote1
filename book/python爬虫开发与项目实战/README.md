# 多进程：multiprocessing，我更想使用subprocess
multiprocessing有线程池multiprocessing.Pool，可以控制同时运行的线程的最大数量；还有multiprocessing.Queue，可以通过此的get或put进行进程间通信；使用managers子模块可以实现分布式。
# 多线程(继承Threading.Thread类)，
线程同步：如果需要保证多线程安全，可以创建一个线程锁threading.RLock()对象，这个对象的acquire()方法和release()方法之间的代码都是安全的。
# python推荐使用多进程，而不是多线程，因为Python有GIL全局解释器锁，即多线程操作时也只是利用了一个CPU内核。故：
CPU密集型操作：使用多进程。
IO密集型操作：使用多线程。
# 网络编程 socket，

