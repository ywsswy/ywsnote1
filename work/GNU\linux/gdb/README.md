gdb main.o //启动gdb调试程序 或者 gdb 进入后 file main.o// 在哪个目录gdb，fopen的相对路径就是哪里，非gdb的时候也是哪个目录启动，就从哪里相对路径，反正不以二进制所在目录为准，而是以用户所在目录为准（./走的那种）
-p <pid> 是可以调试正在运行中的进程，attach成功时进程会卡住，所以需要快速敲完命令然后detach（可以不让进程卡住）再quit（进程不会退出只是gdb退出），如果担心手速不够快可以写好脚本然后直接gdb -x <脚本>执行；

## 通用 ~/.gdbinit
#保持命令历史，每次第一条命令都是这个
set history save on
set print pretty on
#打印全部字符串内容
set print element 0
#调试多线程
set detach-on-fork off
#把输出定向到其他处
tty /dev/pts/0

## 每个程序不同的部分
inferior <num> 切换到某个线程来调试
set logging file <file name> #设置输出会同步到某文件
set logging on/off

disp[lay] <var> //加入watch列表，每次程序停下来都会输出一次，undisplay关闭；如果是寄存器，则是$<var>
b[reak] yPrint //在yPrint函数第一行设置断点
b[reak] 23 //在第23行设置断点
b[reak] <path>/<file.cc>:23
i[nfo] b[reak]/files/ //查看断点/加载表
kill //终止运行，下次可以r来重新运行
winheight SRC</asm/regs> -4 //调整代码窗口高度
q //退出gdb
layout src/asm //显示源代码/汇编窗口
show disassembly-flavor  // 查看汇编窗口的语法
disa[ssemble] main 反汇编main函数
disab[le] <1 //关闭1号断点，enable 打开
Ctrl+x a //不显示代码窗口
Ctrl+x o //切换窗口焦点
Ctrl+l //刷新layout，用来处理花屏问题
r[un] //开始调试，会在断点处停下 r <args> < file //启动参数以及某文件作为标准输入 重定向
n //step over(src)
ni //step over(asm)
s //step into(src)
si //step into(asm)
fin[ish] //step out
c[ontinue]
clear 23 //清除23行的断点
del[ete] 1 //删除1号断点
i[nfo] r[egisters] //显示所有汇编寄存器变量的值
info threads  // 显示所有线程
thead <num> // 切换到第几个线程，然后看bt，可以定位多线程问题
bt显示程序调用栈，也可以用此查看下一条执行哪行语句
pt[ype] 显示变量的详细类型信息
dir <绝对路径> 可以指定加载源文件的路径
catch throw  # 使用gdb捕获异常的扔出点（相当于在扔出异常的地方添加断点）

如果发现step_into无法进入函数，有可能优化不够，也有可能是个宏定义，编译的时候要-g3 -O0，这样可以通过macro expand force_string(arg)来看一个语句背后的语句

wha[tis]  # 显示变量的类型
p 显示变量的值
p * <var>._M_ptr  # 看智能指针的值

p *(<type>*)<address>

x/32xb 0x7fffffffdd70    # 从0x7fffffffdd70开始，显示32个字节的数据
0x7fffffffdd70: 0x03    0x00    0x00    0x00    0x04    0x00    0x00    0x00
0x7fffffffdd78: 0x05    0x00    0x00    0x00    0x06    0x00    0x00    0x00
0x7fffffffdd80: 0x07    0x00    0x00    0x00    0x00    0x00    0x00    0x00
0x7fffffffdd88: 0x00    0x04    0x40    0x00    0x00    0x00    0x00    0x00

dump binary memory log.txt yy_ec yy_ec+256 以二进制的形式把内存打印到文件中

## gdb发现断点行数/文件不对
可能是因为有这个宏
#line 3 "lex.yy.c"#表示从这行开始算是lex.yy.c文件的第3行（如果在下一行打印__LINE__的话，则是4）

## 配置core文件的命名：
/proc/sys/kernel/core_pattern