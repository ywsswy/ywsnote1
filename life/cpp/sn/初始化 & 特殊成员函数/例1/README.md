```
class BBB {
 public:
  int b1{1};
  int b2{2};
};

class AAA {
 public:
  int v1{4};
  int v2{5};
  BBB b1;
  int v5{8};
  int v6{9};
};
void Fun() {
  AAA a;
}
这段函数的汇编如下，相当于编译器给A类和B类生成的默认构造函数，并不是真实存在的函数，即不会生成构造函数的符号，而是直接在函数栈内对类的成员做了初始化（可以理解成内联优化了）；如果BBB b1换成了BBB* b1也是不会生成构造函数符号的；但是如果换成AAA a = AAA{};或者AAA* a = new AAA();则会生成_ZN3AAAC1Ev符号；
00000000004007bb <_Z3Funv>:
  4007bb:       55                      push   %rbp
  4007bc:       48 89 e5                mov    %rsp,%rbp
  4007bf:       48 83 ec 30             sub    $0x30,%rsp
  4007c3:       c7 45 d0 04 00 00 00    movl   $0x4,-0x30(%rbp)
  4007ca:       c7 45 d4 05 00 00 00    movl   $0x5,-0x2c(%rbp)
  4007df:       c7 45 e0 01 00 00 00    movl   $0x1,-0x20(%rbp)
  4007e6:       c7 45 e4 02 00 00 00    movl   $0x2,-0x1c(%rbp)
  4007f4:       c7 45 ec 08 00 00 00    movl   $0x8,-0x14(%rbp)
  4007fb:       c7 45 f0 09 00 00 00    movl   $0x9,-0x10(%rbp)
  40081a:       c9                      leaveq 
  40081b:       c3                      retq
```