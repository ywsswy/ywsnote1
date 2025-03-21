条件变量用于多线程中，线程需要“等”另外线程结束（通知）后才能继续执行的场景，即某些数据有前置依赖；


例如B、C……线程都要等A线程计算完 int x 后才能执行；

#include <condition_variable>
#include <mutex>

std::condition_variable cv;
std::mutex mutex;  // A要写x，B/C要读x，所以一定要保证x的原子性；
int x;

void A() {
  int tmp = xxx();  // 比较耗时
  std::unique_lock<std::mutex> lock(mutex);
  x = tmp;
  lock.unlock();  // 等待方的cv肯定是和mutex同级，因为等待方恢复运行前需要重新加锁（等待方的cv自动行为），所以通知方notify_all执行前必须先释放锁（通知方的主动显式行为）；
  cv.notify_all();
}

void B() {
  std::unique_lock<std::mutex> lock(mutex);
  // wait执行的同时会立刻“自动”解锁
  // notify之后或者超时之后，恢复运行前会“自动”重新加锁（相当于考虑到接下来要操作x了）
  if (cv.wait_for(lock, std::chrono::seconds(1)) != std::cv_status::timeout) {
    // 说明A线程调用了notify_all
    // 读取x
  } else {
    // 说明超时了
  }
}

// C() 同 B()，不过注意的是，notify_all虽然同时通知到了B和C，但是B和C无法同时执行，只有先执行的释放了lock之后，后面的才会执行；