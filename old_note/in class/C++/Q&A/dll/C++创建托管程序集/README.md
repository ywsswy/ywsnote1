新建-》模版-》C++-》CLR（面向公共语言运行时 (CLR)）-》类库-》0414clr1
解决方案资源管理器-属性-C/C++-预编译头-不使用预编译头
//.h
#pragma once
//#include
//#pragma comment(lib,"wininet")
using namespace System;
namespace My0414clr1 {
	public ref class Class1
	{
	public://切记public
		int ymul(int a, int b);
		void print();
	};
}
//////////////////////////////////////////////////////////////////////
//.c
// 这是主 DLL 文件。
#include "stdafx.h"
#include <iostream>
#include "0414clr1.h"
namespace My0414clr1{
	int Class1::ymul(int a, int b){
		return a*b;
	}
	void Class1::print(){
		std::cout << "hello" << std::endl;
	}
}
//////////////////////////////////////////////////////////////////////
生成解决方案
获取.dll文件给C#使用【详见C#使用c++类库】

