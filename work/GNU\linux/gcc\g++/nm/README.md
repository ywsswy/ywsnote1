# 盲猜每一种符号的含义：
v       void
i       int
j       unsigned int
l       long
m       unsigned long
c       char
h       unsigned char
f       float
d       double
Ss      std::string
4Base   Base
4Base   Base
P4Base  Base*
7Derived        Derived
7Derived        Derived

[hill@PfAaUU2029 test]$ nm apple.o  // .a文件也可以
000000000000002a T _ZN5Apple8GetColorEv # T表示代码段，_ZN<类名的长度：5><类名：Apple><函数名的长度：8><函数名：GetColor>E<形参类型：void>
0000000000000014 T _ZN5Apple8SetColorEi
0000000000000000 T _ZN5AppleC1Ev #？这是构造函数？
0000000000000000 T _ZN5AppleC2Ev
[hill@PfAaUU2029 test]$

[hill@PfAaUU2029 test]$ nm AppleWrapper.o 
0000000000000086 T GetColor # 这个像是c编译器搞出来的
0000000000000000 T GetInstance
                 U __gxx_personality_v0
000000000000003c T ReleaseInstance
0000000000000064 T SetColor
                 U _Unwind_Resume
                 U _ZdlPv
                 U _ZN5Apple8GetColorEv # U表示未定义，虽然编译过了，但是没有实现，需要等到链接时去其他文件找实现
                 U _ZN5Apple8SetColorEi
                 U _ZN5AppleC1Ev
0000000000000000 W _ZN8tagAppleC1Ev
0000000000000000 W _ZN8tagAppleC2Ev
0000000000000000 n _ZN8tagAppleC5Ev #这个又像是c++编译器搞出来的，所以可以猜测这个.o可能是使用了extern "C" { 包了一层c++代码 }
                 U _Znwm
[hill@PfAaUU2029 test]$ 

- 从函数符号里看不出来返回值类型，因为程序中不可能同时存在 int Fun1() 和 double Fun1()，所以只要编译器保证这一点就可以不需要把返回值放进符号里了