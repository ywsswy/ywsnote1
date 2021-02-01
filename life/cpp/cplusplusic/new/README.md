## 2.2 头文件保护
使用 #define 来防止头文件被多重包含, 命名格式当是: <PROJECT>_<PATH>_<FILE>_H_（me：如果没定下project，就_<PATH>_<FILE>_H_)
#ifndef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_
...
#endif // FOO_BAR_BAZ_H_

## 7.5 静态static变量 全局变量 const常量
const type kValue = xxx;
## 8.1 注释
//更常用
## 8.2 文件注释
// Copyright <year> <owner> All rights reserved.
// Author <mail>
// Date 2021/01/2
//
// 其他文件注释
// ...
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