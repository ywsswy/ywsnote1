多线程的时候才需要考虑锁

### 互斥锁
共享变量 i ，有多个线程都可能访问，一个线程加了互斥锁之后，其他线程就没法继续加锁了，只能【等待】那个线程解锁后，其他线程才能加锁然后处理
```
std::mutex mutex_本身可以进行lock()和unlock()来保护其内的操作是原子的，这样多线程修改一个变量就可以是符合预期的
std::unique_lock<std::mutex> guard(mutex_)可以为了防止忘记遗漏执行unlock带来的死锁（另一个线程就一直等不到了），相当于用构造&析构函数来实现语句块内加锁； // c++17开始可以不显式指定模板类型
std::lock_guard同unique_lock，不过没有多余的接口（没有lock/unlock接口），构造函数时拿到锁，析构函数时释放锁，性能更极致；
```

### 读写锁
多个读者可以同时进行读
其他情况均互斥（写锁时，不允许再写锁和读锁；读锁时，也不允许再写锁）
写者优先于读者（一旦有写者，则后续读者必须等待，唤醒时优先考虑写者）
```
std::shared_mutex mutex_;
然后Set函数里用，独占的锁
std::unique_lock<std::shared_mutex> guard(mutex_);
Get函数里用，可共享的锁
std::shared_lock<std::shared_mutex> guard(mutex_);  // 执行到这一行代码的时候才加锁，语句块退出的时候自动解锁
```

```
#include <iostream>
#include <thread>
#include <mutex>
using namespace std;
int g_count = 0;

void mythread1(mutex& m) {
 for (int i = 0; i < 10000; i++) {
  m.lock();
  g_count++;
  m.unlock();
 }
}

int main() {
   mutex m;
   std::thread t1(mythread1, std::ref(m));
   std::thread t2(mythread1, std::ref(m));
   t1.join();
   t2.join();
   cout << "实际是" << g_count << endl;
}
```


