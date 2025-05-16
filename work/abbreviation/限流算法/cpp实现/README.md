#include <atomic>

// 令牌桶实现，最多支持100W的QPS
class Limiter {
public:
 explicit Limiter(int rate) : last_time_(NowUs()), per_token_(1000000L / rate) {}
 bool TryGet(int tokens = 1) {
  long long now = NowUs();
  int needs = tokens * per_token_;
  long long expected = last_time_.load(std::memory_order_relaxed);

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
 void SetRate(int rate) {
  per_token_ = 1000000L / (rate + 1);
 }

 private:
  std::atomic<long long> last_time_;
  int per_token_;
};