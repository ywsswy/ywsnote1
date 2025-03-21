class PendingCacheValue {
 public:
  bool done{false};  // false: 首次请求占位的cache，但是还没有处理完; true: 已处理完毕可使用的正常cache
  uint64_t update_ms{0};
  Mutex mutex;
  ConditionVariable cond;
  Value v;
};

class PendingCache { // 单例
 public:
  std::shared_ptr<PendingCacheValue> GetOrMakeCache(const std::string& cache_key, bool& need_update) {
    std::lock_guard lock(mutex_);
    auto&& iter = cache_->find(cache_key);
    if (iter == cache_->end()) {
      std::shared_ptr<PendingCacheValue> pc = std::make_shared<PendingCacheValue>();
      cache_->set(cache_key, pc);
      need_update = true;
      return pc;
    } else {
      std::lock_guard lock(iter->second->mutex);
      if (iter->second->done &&
          iter->second->update_ms + pending_cache_expire_ms < NowMs()) {
        need_update = true;
        iter->second->done = false;
      }
      return iter->second;
    }
  }

 private:
  Mutex mutex_;
  std::map<std::string,PendingCacheValue> cache_;
};


Value RealFun1(std::string key); // 比较耗时的逻辑，同一瞬间多个相同请求，其实只需要一个做，其他等这个完事直接使用结果即可；

Value Fun1(std::string key) {
  bool need_update = false;
  std::shared_ptr<PendingCacheValue> pending_cache = PendingCache::GetInstance()->GetOrMakeCache(key, need_update);
  // 第一个请求，或者已经过期的请求
  if (need_update) {
    auto v = RealFun1(key);
    std::lock_guard lock(pending_cache->mutex);
    pending_cache->v = v;
    pending_cache->done = true;
    pending_cache->update_ms = NowMs();
    pending_cache->cond.notify_all();
    return v;
  }
  // 后续请求，如果缓存中数据可用直接使用，否则等待缓存可用
  {
    std::unique_lock lock(pending_cache->mutex);
    if (pending_cache->done) {
      return pending_cache->v;
    }
    if (pending_cache->cond.wait_for(lock, std::chrono::milliseconds(pending_cache_timeout_ms)) ==
        std::cv_status::no_timeout) {
      // notify的话，肯定是处理完的cache，不会被修改了，可以直接读取
      return pending_cache->v;
    }
  }
  // 等待超时的话，则不等待了
  return RealFun1(key);
}