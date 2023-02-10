11:21
41个视频
12：00
30个视频


3[idx] //这也可以访问数组
int a[12] = {[2] = 96}; //还可以这么指定初始化

sizeof(数组);//占用的总字节

gcc -E test.c -o test.i #预处理文件（宏展开）
gcc -S test.i -o test.s #得到汇编
gcc -c test.s -o test.o #得到可重定位的目标文件，很接近可执行文件了，但是这个文件应该被加载到内存什么地方，还需要经过链接。（使用nm命令查看.o文件可以判断是c编译的还是cpp编译的，因为c不支持重载，所以函数名独立，但是cpp里面都是加了下划线各种独立）
gcc test.o 

objdump -d test.o #反汇编查看目标文件
gcc -o bin main.c test.o #main.c中含有main函数了，这种组合起来就会把目标文件中的可重定位的地址放到正确地址

linux下二进制程序文件是ELF格式
程序（段）头表
.init节
.text节
.rodata节
.data节
.bss节
.symtab节
.debug节
.line节
.strtab节
运行加载到计算机中是分别放到
低地址
只读代码段（.init .text .rodata）
独写数据段（.data .bss）
堆中
栈（用户栈由高地址向低地址增长，增长的边界=用户栈顶ESP）
高地址