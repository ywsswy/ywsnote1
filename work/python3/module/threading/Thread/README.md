import threading
import time

class YGlobal(object):
    global_flag = True


def f1(no, count):
    for i in range(0, count):
        print("sleep {}".format(no))
        time.sleep(1)
        YGlobal.global_flag = False

thread1 = threading.Thread(target=f1, args = (1,6) )
thread1.start()
thread1.join()
print("end")
print(YGlobal.global_flag)