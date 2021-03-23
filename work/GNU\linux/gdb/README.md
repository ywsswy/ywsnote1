gdb main.o //启动gdb调试程序 或者 gdb 进入后 file main.o// 在哪个目录gdb，fopen的相对路径就是哪里，非gdb的时候也是哪个目录启动，就从哪里相对路径，反正不以二进制所在目录为准，而是以用户所在目录为准（./走的那种）

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

disp[lay] <var> //加入watch列表，每次程序停下来都会输出一次，undisplay关闭
b[reak] yPrint //在yPrint函数第一行设置断点
b[reak] 23 //在第23行设置断点
i[nfo] b[reak]/files/ //查看断点/加载表
kill //终止运行，下次可以r来重新运行
winheight SRC</asm/regs> -4 //调整代码窗口高度
q //退出gdb
layout src //显示源代码窗口
disa[ssemble] main 反汇编main函数
disab[le] <1 //关闭1号断点
Ctrl+x a //不显示代码窗口
Ctrl+x o //切换窗口焦点
Ctrl+l //刷新layout，用来处理花屏问题
r[un] //开始调试，会在断点处停下 <args> r < file //启动参数以及某文件作为标准输入 重定向
n //step over(src)
ni //step over(asm)
s //step into(src)
si //step into(asm)
c[ontinue]
clear 23 //清除23行的断点
del[ete] 1 //删除1号断点
i[nfo] r[egisters] //显示所有汇编寄存器变量的值
bt显示程序调用栈，也可以用此查看下一条执行哪行语句
fin[ish] //step out
p 显示变量的值
wha[tis] 显示变量的类型
pt[ype] 显示变量的详细类型信息
dir <绝对路径> 可以指定加载源文件的路径



如果发现step_into无法进入函数，有可能优化不够，也有可能是个宏定义，编译的时候要-g3 -O0，这样可以通过macro expand force_string(arg)来看一个语句背后的语句
