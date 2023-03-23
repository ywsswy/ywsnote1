https://github.com/google/benchmark/blob/main/docs/user_guide.md


Q：
- 这个输出格式：
```
Running ./a.out
Run on (16 X 2595.12 MHz CPU s)
CPU Caches:
  L1 Data 32 KiB (x8)
  L1 Instruction 32 KiB (x8)
  L2 Unified 4096 KiB (x8)
  L3 Unified 16384 KiB (x2)
Load Average: 0.05, 0.03, 0.05
-----------------------------------------------------
Benchmark           Time             CPU   Iterations
-----------------------------------------------------
BM_foo      508756529 ns      8679215 ns           10
BM_bar        4650630 ns      4650592 ns          150
```
其中前面部分表示机器的信息，
Iterations是函数被迭代的次数（是benchmark为确保结果统计稳定而动态决定的，显示的不一定是实际的，可能比实际略少几次）；
"Time" 表示每次函数调用的平均执行时间，单位是纳秒（ns），反映了函数的总体执行速度；
"CPU" 则表示每次函数调用实际消耗的 CPU 时间，也是以纳秒为单位，并不能完全反映函数的性能表现，但是CPU越短肯定并发能力更强；
ps:
1秒（s）= 1000毫秒（ms）
1毫秒（ms）= 1000微秒（μs）
1微秒（μs）= 1000纳秒（ns）