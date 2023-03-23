#include <vector>
#include <iostream>
#include <unistd.h>

void foo() {
    std::vector<int> vec(1000000, 1);
    for (auto& x : vec) {
        x += 1;
    }
    //std::cout << "foo()" << std::endl;
}

void bar() {
    std::vector<int> vec(1000000, 1);
    for (size_t i = 0; i < vec.size(); ++i) {
        vec[i] += 1;
    }
    //std::cout << "bar()" << std::endl;
}


#include <benchmark/benchmark.h>

static void BM_foo(benchmark::State& state) {
  for (auto _ : state) {
    foo();
  }
}
BENCHMARK(BM_foo);

static void BM_bar(benchmark::State& state) {
  for (auto _ : state) {
    bar();
  }
}
BENCHMARK(BM_bar);  // 注册BM_bar函数，然后才会测这个函数的性能

BENCHMARK_MAIN();  // 执行入口main函数

// g++ test.cc -std=c++11 -lbenchmark -lpthread