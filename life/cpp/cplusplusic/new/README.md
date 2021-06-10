还要再细看的：3.5 6.6

## 2.2 头文件保护，跟#pragma once相比，可能更跨平台，但是我选择pragma once
使用 #define 来防止头文件被多重包含, 命名格式当是: <PROJECT>_<PATH>_<FILE>_H_（me：如果没定下project，就_<PATH>_<FILE>_H_)
#ifndef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_
...
#endif // FOO_BAR_BAZ_H_
## 2.5 include的顺序，对xxx.cpp而言
```
xxx.h
A blank line
C system headers
A blank line
C++ standard library headers
A blank line
Other libraries' .h files.
A blank line(optional)
Your project's .h files.
```
## 4.1 构造函数的职责（不允许调用虚函数，<font color=red>应该怎么做</font>)
## 4.9 声明顺序
public: 段 protected: 段，最后是 private: 段
在每一段内顺序： 类型 (包括typedef，using 和嵌套的结构体与类)，常量，<font color=red>工厂函数</font>，构造函数，赋值运算符，析构函数，其它函数，数据成员
## 6.14 
整数用 0，浮点数用 0.0
指针用 nullptr，nullptr 具备类型安全，而 0 和 NULL 通常都是整型，后者在函数重载和模板类型推导时会出问题
## 6.24 别名用 using Bar = Foo; 代替typedef Foo Bar;
## 7.5 静态static变量 全局变量 const常量
const type kValue = xxx;
## 8.1 注释
//更常用
## 8.2 文件注释，考虑到doxygen
```
// Copyright <year> <owner>  All rights reserved.
/// @file <file_name>
/// @author <mail>
/// @date 2021/05/26
/// @brief 说明<br>
/// 其他文件注释（这里要紧跟brief不能留空行，如果要可视化换行，使用br标签）<br>
/// 文件注释必须写对文件名，不然不会被分析，而且最好写全路径，这才能保证唯一，不然两个文件夹里的文件名相同也不会被分析
```
## 8.4 函数注释
 定义处描述函数实现；
 声明处描述功能和用途；
 注释使用叙述式（"Opens the file"）而非指令式（"Open the file"）；
 说“能xxx”，“用于xxx”，而不是“做xxx”；注释不会描述函数如何工作，那是函数定义部分的事情
example:
  // 用于xxx
  // in:
  //    xxx: xxx
  // out:
  //    xxx: xxx
  // return: xxx

## 8.6 行尾的注释符号//要跟前面的代码空两格空格，要跟后面的注释内容空一个空格；namespace 结尾要注释 }  // namespace xxx
## 8.8
// TODO(hill) do something
## 9.1 行长度100，（google 80）
## 9.10
switch (var) {
  case 0: {  // 2 space indent
    ...      // 4 space indent
    break;
  }
## 9.11
 (*, &) 作为指针/地址运算符时之后不能有空格
(*, &)在用于声明指针/引用变量或参数时，建议空格后置