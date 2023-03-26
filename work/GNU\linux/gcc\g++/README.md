```
g++/gcc
  -fPIC  // 生成位置无关代码，不论是生成静态库还是动态库，不论最后是用于静态链接还是动态链接，最好编译阶段都加上，否则后面又可能就会报错：libxxx.a(xxx.o): requires unsupported dynamic reloc 11; recompile with -fPIC
  -g  // 编译并生成带有调试信息
  -O0  // 编译时优化级别为0（不经任何优化）
  -c  // 得到可重定位的relocatable文件
  main.cc  // 编译的文件
  -o main  // 输出的文件名，默认生成executable文件时文件名是a.out
  -std=c++11  // 支持C++11语法
  -Wall  //警告级别为：all
  -I <头文件路径>
  -isystem <标准库头文件路径>
  -iquote <用户自定义头文件路径>
  -L <库文件路径> 
  -lpthread  // 链接需要库
```

1. 对于 *.c和*.cpp文件，gcc（程序）分别【当做】c和cpp文件编译（c和cpp的语法强度是不一样的）
2. 对于 *.c和*.cpp文件，g++（程勋）则统一【当做】cpp文件编译
3. 使用g++编译文件时，g++会自动链接标准库STL，而gcc不会自动链接STL（所以要 -lstdc++）（但这并不代表 gcc –lstdc++ 和 g++等价）
4. gcc在编译C文件时，可使用的预定义宏是比较少的
5. 一个源文件在被【当作】cpp文件时，会加入一些额外的宏，这些宏如下：
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
6. 其他构建细节
gcc -E test.c -o test.i  # 预处理文件（宏展开），更好看的写法： gcc -E -P test.c >test.i，不像深入进入的头文件就直接注释掉，不然可能各种include找不到
gcc -S test.i -o test.s  # 得到低级的汇编
gcc -c test.s -o test.o  # 得到可重定位的relocatable文件，很接近可执行文件了，但是这个文件中的一些函数调用地址经过链接后才能确定
gcc -o a.out test.o  # 把一些relocatable文件（要求其中必须含有main函数）组合（链接）起来得到最终的executable文件
gcc的参数可以写进一个文件里，然后用gcc @<filename>来执行
gcc -print-search-dirs可以查看其搜索的库路径（应该是读的LIBRARY_PATH配置）