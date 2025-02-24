import threading
import time

mutex = threading.Lock()


def test1():
    mutex.acquire()
    print("1")
    time.sleep(3)
    print("1 end")
    mutex.release()


def test2():
    mutex.acquire()
    print("2")
    time.sleep(3)
    print("2 end")
    mutex.release()

threading.Timer(0, test1).start()
threading.Timer(0, test2).start()
while True:
    time.sleep(1)



GIL导致一个进程没法高效利用多核
io密集型最好多线程
cpu密集型最好多进程
Queue是线程安全的
线程同步
```
lock=threading.Lock()
lock.acquire()
dosomething
lock.release()
 
lock=threading.RLock() #可重入锁，即一个函数acquire了，其内再调用的函数也可以acquire
Condition条件变量
Semaphore信号量
```