## 到底需要benchmark提供什么样的能力？平常的性能测试到底有哪些场景？

1）空间内变量值固定，要测试性能的这段代码每次执行前后，空间内变量没有任何改变；
2）空间内变量值有k种情况，要测试每一种情况下这段代码的性能，每种情况下每次执行前后空间内变量没有任何改变；
3）预期一段代码可能会随某变量值不同而性能差异很大，比如不同排序算法复杂度不同，有的平均不错，但是最坏情况可能很差；这时候要随机生成变量的值多跑几种情况；

1）的方案：
```
#include <benchmark/benchmark.h>

class MyFixture : public benchmark::Fixture {
public:
  void SetUp(const ::benchmark::State& state) {
    // 变量初始化逻辑
  }

  void TearDown(const ::benchmark::State& state) {
    // 析构等清理逻辑
  }
  // 这里可以定义一些变量
};

BENCHMARK_F(MyFixture, MyBenchmark) (benchmark::State& state) {
  for (auto _ : state) {  // why auto
    // 需要测试性能的逻辑，可以读到MyFixture中的变量
  }
}

BENCHMARK_MAIN();

```