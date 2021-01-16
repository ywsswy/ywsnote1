# 写在前面：纯粹的C++
python编程讲究pythonic，同样C++也要cplusplusic。不要把C++写得四不像，让人分不清是C还是c++。
# 一些“约定俗成的”代码规范
## 命名
主要参考[google的C++代码规范](https://blog.csdn.net/freeking101/article/details/78930381)
个人整理如下：
* 类型和变量应该是名词性的，函数名可以用“命令性”动词。
* 类、结构体、类型定义（typedef）、枚举、【普通函数】。每个单词首字母大写，无下划线
```
class MyClass : public OtherClass {
 public: // 按照 public 、 protected、private 的顺序进行定义（缩进一个空格）
  MyClass(); // 缩进4个空格(google 2个)
  explicit MyClass(int var);
  ~MyClass() {}

  void SomeFunction();
  void SomeFunctionThatDoesNothing() {}
 private:
  int number_; //成员变量下划线结尾
}
enum UrlTable{ ...
```
```
MyClass::MyClass(int var)//构造函数初始化参数列表
    : some_var_(var),
      some_other_var_(var + 1) 
{
    DoSomething();
}
```
* 访问存取函数。set_num()，小写，单词间下划线
* 变量名。小写，单词间下划线相连，【全局变量以“g_”开头，类的成员变量以“_”结尾】
* 例外：常量前加k：const int kDaysInAWeek；
* 文件名。c++文件已.cc结尾
* 总结：1）类同普通函数；2）不存在驼峰；3）仅变量存在下划线
## 其它
* [google编程规范pdf](http://106.53.5.24:65533/static/file/google%E7%BC%96%E7%A8%8B%E8%A7%84%E8%8C%83C%2B%2B.pdf)
* [google编程规范link](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/comments/)
* 不定义全局变量，不用using namespace污染命名空间
* 使用标准库函数，考虑兼容性，例如不使用C++11中才有的auto关键字，不使用GNU才有的__gcd()函数。
* 使用class代替struct
* 使用安全的类型和函数，比如：
```
使用vector，不使用数组
使用std::cin和std::cout，不使用scanf和printf
使用stringstream，不使用sprintf
...
```
* 其中输入输出部分，让C++更纯粹，脱离C的速度累赘，使用如下代码加速
```
std::cin.tie(0);
std::ios::sync_with_stdio(false);
//std::cout<<'\n' 代替 std::cout << std::endl;
```
* 赋值比自增自减快
* ++i比i++快
# 一些“约定俗成的“术语
## 1s 和 0s
1s表示从1开始（1 start），比如下面
```
std::vector<int> ve;//ve(1s)
```
表示ve[1]是第一个元素，而ve[0]是没用的。同理0s则表示从ve[0]开始存第一个元素。

# 头文件
使用 #define 来防止头文件被多重包含, 命名格式当是: <PROJECT>_<PATH>_<FILE>_H_（me：如果没定下project，就_<PATH>_<FILE>_H_
#ifndef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_
...
#endif // FOO_BAR_BAZ_H_

## include的顺序，对xxx.cpp而言
xxx.h
本项目内 .h 文件
其他库的 .h 文件
C++ 系统文件
C 系统文件