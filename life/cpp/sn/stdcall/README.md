不同编译器处理栈的方式不同：
1)多个参数时，按照什么顺序入栈
2)当函数调用完毕后，交给谁来做栈的清除恢复工作

所以函数调用需要有个约定

stdcall // pascal
cdecl
fastcall
thiscall


声明语法为
int __stdcall function(int a, int b)
表示：
1)该函数参数从右向左入栈
2)交给函数自身修改堆栈
3)编译时名称变为 _function8 表示结束后ret 8清理8个字节的堆栈


默认是声明为
int __cdecl function(int a, int b)
1)该函数参数从右向左入栈
2)交给调用者修改堆栈 结束后会add esp, 8
