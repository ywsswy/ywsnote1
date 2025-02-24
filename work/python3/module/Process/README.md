# 只有用多进程才能实现cpu利用率大于100%
from multiprocessing import Process

def pwn(start, end):
  for a1 in range(start, end) :
    pass

p1 = Process(target=pwn, args=(48, 58))
p2 = Process(target=pwn, args=(58, 68))
p1.start()
p2.start()
p1.join()
p2.join()