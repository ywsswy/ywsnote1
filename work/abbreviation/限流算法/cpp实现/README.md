#include <atomic>

// 令牌桶实现，最多支持100W的QPS
class Limiter {
public:
 explicit Limiter(int32_t rate) : last_time_(NowUs()), per_token_(1000000L / rate) {}
 bool try_get(int32_t tokens = 1) {
  int64_t now = NowUs();
  int32_t needs = tokens * per_token_;
  int64_t expected = last_time_.load(std::memory_order_relaxed);

  int64_t min_desired = now - 1000000L;  // 长时间没流量，或者流量少时，允许瞬时QPS达到设置的最大值
  do {
    int64_t desired = expected + needs;
    if (desired < min_desired) {
      desired = min_desired;
    }

    if (desired > now) {
      return false;  // 被限流
    }

    if (last_time_.compare_exchange_weak(expected, desired, std::memory_order_relaxed, std::memory_order_relaxed)) {
      return true;
    }
  } while (true);

  return false;
}

 private:
  std::atomic<int64_t> last_time_;
  int32_t per_token_;
};