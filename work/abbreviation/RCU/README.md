Read-Copy-Update (RCU)是一种用于实现并发访问共享数据结构的技术，它是一种无锁的读取更新方式
通常使用std::atomic<T*> 和 load(std::memory_order_acquire) 实现

