https://blog.csdn.net/u011195077/article/details/121122530

```
import multiprocessing
import time

def func(msg):  # 多进程之间的变量不是共享的，哪怕是全局变量，所以如果需要一个进程修改，其他进程感知到，必须使用多进程库multiprocessing.Manager()
    # 注意多进程的异常处理，一个进程里面的exit外面是感知不到的
    print("msg:", msg)
    time.sleep(1)
    print("end")

if __name__ == "__main__":
    with multiprocessing.Pool(processes=1) as thred_p:
      res_arr = []
      for i in range(10):
        print("here")
        msg = "hello %d" %(i)
        res = thred_p.apply_async(func, (msg, ))
        res_arr.append(res)

      # 瞬间就执行到这里了，因为apply_async不阻塞；但是虽然执行到这里（进程提交了），但是并不是所有进程同时运行的，前面设置了processes=1，表示最多只有一个进程在运行，一个执行完，才会执行下一个
      for j in range(100):
        print("sleep")
        time.sleep(1)
      for tmp_res in res_arr:
        tmp_res_arr = tmp_res.get()  # 因为with的退出并不会执行thred_p.join，所以这里可以使用get等待进程结束
        print("线程返回：{}".format(tmp_res_arr))

    time.sleep(3)
    print("Mark~ Mark~ Mark~~~~~~~~~~~~~~~~~~~~~~")
    print("Sub-process(es) done.")
```