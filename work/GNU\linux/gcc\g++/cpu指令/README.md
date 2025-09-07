- 生成包含avx512指令的程序
g++ main.cc -mavx512f

只是告诉编译器可以生成，但是如果代码里没有用到特殊的代码，可能生成也只是普通的cpu指令就够了；




- 判断一个机器支持哪些cpu指令
lscpu |grep Flags

仅“运行时”有要求，二进制中存在当前机器不支持的cpu指令时，会报错：Illegal instruction (core dumped)
“编译时”无所谓，就算当前机器不支持运行时指令 但不影响编译器构建出来对应的指令；


- SIMD
SIMD（Single Instruction, Multiple Data） 是一种并行计算技术，允许单条指令同时对多个数据进行操作；
SSE、AVX、AVX2、AVX-512 是 SIMD（Single Instruction, Multiple Data）指令集的一部分；
SSE 可操作128位寄存器（XMM）
AVX/AVX2 可操作256位寄存器（YMM）；机器上可能有16个YMM寄存器：YMM0到YMM15
AVX512 可操作512位寄存器（ZMM）；
c++代码：
```
__m256 浮点数变量（存8个float32）
__m256i 整数变量（存8个int32）
sum = _mm256_fmadd_ps(a,b,sum)  // a和b中的8个浮点数逐个相乘，结果加到sum里（sum存的也是8个浮点数）
```

判断是否生成了包含avx512指令的可以objdump -D后搜索zmm，其他同理；（只不过生成了不代表代码会运行到那里，所以还是要实际去机器运行。。）