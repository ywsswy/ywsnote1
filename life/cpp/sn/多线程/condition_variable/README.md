条件变量用于多线程中，线程需要“等”另外线程结束（通知）后才能继续执行的场景，即某些数据有前置依赖；


例如B、C……线程都要等A线程计算完 int x 后才能执行；

#include <condition_variable>

std::condition_variable cv;
std::mutex mutex;  // 保证x的原子性
int x;

void A() {
  int tmp = xxx();  // 耗时
  {
    std::lock_guard<std::mutex> lock(mutex);
    x = tmp;
  }
  cv.notify_all();  // 等待方的cv肯定是和mutex同级，等待方恢复运行前需要重新加锁，所以即使notify_all放在里面也是必须要等锁释放互斥量后才能恢复等待方的运行
}

void B() {
  std::unique_lock<std::mutex> lock(mutex);
  // wait执行的同时会立刻“自动”解锁
  // notify之后或者超时之后，恢复运行前会“自动”重新加锁（相当于考虑到接下来要操作x了）
  if (cv.wait_for(lock, std::chrono::seconds(1)) != std::cv_status::timeout) {
    // 说明A线程调用了notify_all
  } else {
    // 说明超时了
  }
}

// C() 同 B()