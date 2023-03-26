【生成】
新建Win32项目-》DLL项目（YWSLIB)
导出符号-》空项目
属性-》c/c++预编译头（不使用）
ywslib.h
//////////////////////////////////////////////////////////////////////
//#pragma once
#ifdef YWSLIB_EXPORTS
#define YWSLIB_API __declspec(dllexport)
#else
#define YWSLIB_API __declspec(dllimport)
#endif
// 导出类
class YWSLIB_API Cdll {
public:
	Cdll(void);
};
//导出变量，变量在.cpp文件中定义
extern YWSLIB_API int ndll;
//导出函数，加extern "C",是为了保证编译时生成的函数名不变，这样动态调用dll时才能正确获取函数的地址
extern "C" YWSLIB_API int fndll(void);
extern "C" YWSLIB_API int ywsf(int a, int b);
//////////////////////////////////////////////////////////////////////
ywslib.cpp
//////////////////////////////////////////////////////////////////////
#include "YWSLIB.h"
YWSLIB_API int ndll = 520;
Cdll::Cdll()
{
	return;
}
YWSLIB_API int fndll(void)
{
	return 250;
}
int ywsint = 3;
int ywsf(int a, int b){
	return a + b;
}
//////////////////////////////////////////////////////////////////////
生成解决方案
debug目录下获取.dll和.lib，项目文件夹下获取.h
【分类】
静态库：lib（包含代码本身（实现、索引））
编译时直接将代码加入程序中
动态库：lib+dll
lib包含函数所在dll文件和文件中函数位置的信息  
运行时才会使用到dll
总之，lib是编译时用到的，dll是运行时用到的。
