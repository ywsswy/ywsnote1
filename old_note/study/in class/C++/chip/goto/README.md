//goto只能在本函数内
goto是本地的：它只能跳到所在函数内部的标号上
C函数库提供了setjmp()和longjmp()函数，它们分别承担非局部标号和goto作用。头文件<setjmp.h>申明了这些函数及同时所需的jmp_buf数据类型
setjmp(j)设置“jump”点，用正确的程序上下文填充jmp_buf对象j。这个上下文包括程序存放位置、栈和框架指针，其它重要的寄存器和内存数据。当初始化完jump的上下文，setjmp()返回0值
以后调用longjmp(j,r)的效果就是一个非局部的goto或“长跳转”
#include <setjmp.h>
#include <stdio.h>
#include <windows.h>
jmp_buf j;
void fun()
{
	printf("fun\n");
	Sleep(2222);
	longjmp(j, 3);
}
int main(void)
{
	int a = 0;
	a = setjmp(j);
	printf("%d\n",a);
	Sleep(2222);
	fun();
	return 0;
}
