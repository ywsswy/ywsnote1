makefile编写
exe=main
obj=main.o
$(exe):$(obj)
        g++ -o $(exe) $(obj)#注意这前面是tab键，不是空格
main.o:main.cc
        g++ -c main.cc
clean:
        rm -rf *.o main
"每次重新生成必须要clean
**********************************************************
g++/gcc
  -g main.cc //编译并生成带有调试信息
  -o main.o //的运行文件 g++  //默认不加-o会生成a.out
  -std=c++11 //支持C++11
  -Wall //警告级别为：all
  -O0 //优化级别为0（不经任何优化）

1. 对于 *.c和*.cpp文件，gcc分别【当做】c和cpp文件编译（c和cpp的语法强度是不一样的
2. 对于 *.c和*.cpp文件，g++则统一【当做】cpp文件编译
3. 使用g++编译文件时，g++会自动链接标准库STL，而gcc不会自动链接STL（所以要 -lstdc++）（但这并不代表 gcc –lstdc++ 和 g++等价）
4. gcc在编译C文件时，可使用的预定义宏是比较少的
5. g++ test.cc -std=c++11 -I <头文件路径> -L <库文件路径> -lbenchmark -lpthread
6. 一个源文件在被【当作】cpp文件时，会加入一些额外的宏，这些宏如下：
#define __GXX_WEAK__ 1
#define __cplusplus 1
#define __DEPRECATED 1
#define __GNUG__ 4
#define __EXCEPTIONS 1
#define __private_extern__ extern


#ifdef __cplusplus
    printf("当前程序被编译器认为是C++程序\n");
#else
    printf("当前程序被编译器认为是C程序\n");
#endif