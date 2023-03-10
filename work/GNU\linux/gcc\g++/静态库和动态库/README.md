# 编译阶段
```
gcc -fPIC -c library.cpp -o library.o
gcc -fPIC -c library-bridge.cpp -o library-bridge.o

-fPIC 生成位置无关代码，不论是生成静态库还是动态库，不论最后是用于静态链接还是动态链接，最好编译阶段都加上，否则后面又可能就会报错：
libxxx.a(xxx.o): requires unsupported dynamic reloc 11; recompile with -fPIC
```

# 制作库阶段（-shared参数在制作库阶段才可能用）
```
ar rcs lib6.a library-bridge.o library.o # 制作静态库，注意 .o 或 .a 的顺序是非常重要的，某个 .o 文件只能调用在它后面列出的 .o 文件
gcc -shared -o lib6.so library-bridge.o library.o # 制作动态库

# 通过file命令可以判断一个库文件的类型，ps: ELF（二进制程序文件的格式）有三种类型的文件：relocatable可重定位目标文件（一般是.o）、executable可执行文件、shared共享文件(.so)
> file library.cpp 
library.cpp: C source, ASCII text
> file library.o 
library.o: ELF 64-bit LSB relocatable, x86-64, version 1 (SYSV), not stripped
> file lib5.a
lib5.a: current ar archive
> ar -t lib5.a # 查看静态库中包含的.o文件
library-bridge.o
library.o
> file lib5.so 
lib5.so: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, BuildID[sha1]=50baaa1150fa3284d0a6238faf2af99f20a1408e, not stripped
> file cpptest/a.out 
cpptest/a.out: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=f5b596e43ab8666460095fc891c1a5a336133bcb, for GNU/Linux 3.2.0, not stripped

> ar -xv lib5.a # 提取出静态库中的.o文件
x - library-bridge.o
x - library.o
```

# 链接/使用阶段（-static参数在链接阶段才可能用）
- 不加-static的时候，gcc main.cpp -L./ -lx，优先链接libx.so，如果不存在再链接libx.a，只看文件后缀，文件类型校验是静态库或者动态库均可；
- 加-static的时候，gcc main.cpp -L./ -static -lx，只会链接libx.a，文件类型校验必须是静态库；
- 也可以指定仅一部分-static，命令是gcc main.cpp -L./ -Wl,-Bstatic -lx -Wl,-Bdynamic -lstdc++ # ps: -Wl后面的东西是作为参数传递给链接器ld；

对静态库的处理是，将需要的二进制代码都“拷贝”到可执行文件中(注意, 只拷贝需要的,不会全部拷贝)
对动态库的处理是，仅仅“拷贝”一些重定位和符号表信息，这些信息是在程序运行时完成真正的链接过程

# 注意对外（使用方）提供的接口需要封装到C风格的头文件中，示例（使用方只能使用C风格的SaveIndex函数，至于这个函数里使用了什么c++方法，使用方就不用感知了）：
```
#pragma once
#ifdef __cplusplus
extern "C" {
#endif

int SaveIndex(void* ptr);

#ifdef __cplusplus
}  // extern "C"
#endif
```


# other
- bazel 目前不支持制作静态库。。。所以只能用cc_binary加上参数linkshared = True来制作动态库，如果非要静态库的话，一个不太好用的workaround：https://gist.github.com/oquenchil/3f88a39876af2061f8aad6cdc9d7c045
# Q?
gcc main.cpp可以拆分成两步：gcc -c main.cpp;gcc main.o
- 如果gcc没有加-shared参数也没有加-c参数，那么就会当作是生成可执行文件，所以必须要找到main的实现才行