```
template <class T>
class DataHolder : public Singleton<DataHolder<T> > {
 public:
  std::shared_ptr<const T> GetData() {
    RLock lock(lock_);
    return ptr_;
  }
  void SetData(std::shared_ptr<T> ptr) {
    WLock lock(lock_);
    ptr_ = ptr;
  }

 private:
  RWLocker lock_;
  std::shared_ptr<T> ptr_;
};
```
## 场景：一个单例类，有一些线程要读取使用，有一些线程要写更新
### 必须满足如下条件：
- 1、读取是指针，因为要防止大内存拷贝
- 2、取出去的不允许修改，因为要避免多个线程取出去一个改了影响另一个
- 3、写更新不能原地修改，因为要避免读受影响

关于（2）这里只能用const保证自己持有的T的值不被修改，但是T内的指针成员拷贝后是又可能被修改的，这就要求T自行保证（一个有指针成员的类也理应具备这样的素质）拷贝安全，要么自己实现拷贝，要么禁用拷贝：
```
public:
 A(const A&) = delete;
 A& operator=(const A&) = delete;
```
