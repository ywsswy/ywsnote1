since C++17

#include <string_view>

没有.c_str()方法，但是有.data()方法获取const char*

当函数需要接收一个只读字符串时，使用std::string_view可以替代const string&和const char *

```
void fun2(const char* x) {  // 这种调用端需要.c_str()的写法失去了原本对象std::string良好的面向对象的方法
}
void fun(const std::string& x) {  // 这种写法会有构造函数的开销，换成std::string_view开销就很小
  std::cout << x << std::endl;
}
int main() {
  fun("fweaf");
  fun2(s.c_str());
}
```

返回字符串的函数应该返回const string& 或者string的类型，不应该返回string_view。因为这样可能带来使返回的string_view无效的风险。例如当它指向的字符串需要重新分配时。
即string_view不应该用于保存一个临时字符串。

std::string没有std::string_view参数的构造函数，只有复制赋值运算符；