多线程读写并不会必然导致coredump，例如读写一个int是不会core的，只是会导致值不准确，例如下面的写法实际值会小于20000，因为g_count++这个语句并不是原子的，可能两个线程同时读的时候都是a，然后同时把a+1写回去，实际上应该a+2了
```
#include <iostream>
#include <thread>
using namespace std;
int g_count = 0;

void mythread1() {
 for (int i = 0; i < 100000; i++) {
   g_count++;
 }
}

int main() {
   std::thread t1(mythread1);
   std::thread t2(mythread1);
   t1.join();
   t2.join();
   cout << "实际是" << g_count << endl;
}
```

- atomic的int 就能保证g_count++是原子的？


std::atomic<T> 

bool compare_exchange_strong(T& expected, T desired);

直白理解：
我猜T当前值是expected，
如果我猜对了（返回true），就帮我改成desired，
如果我猜错了（返回false），告诉我它现在到底是啥（通过expected告诉我）