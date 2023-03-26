wtf2.cc: In function ‘int main()’:
wtf2.cc:10:21: error: too many initializers for ‘AAA’
   AAA b{std::move(a)};
                     ^

#include <iostream>

class AAA {
 public:
  AAA() = default;
};

int main() {
  AAA a;
  AAA b{std::move(a)};
  return 0;
}

本意是让AAA类自动生成移动构造函数，但是这个报错是触发了大括号初始化的陷阱；
AAA类被识别成了聚合类（aggregate class），所以大括号初始化会被当做逐个成员赋值，换句话说，如果改成下面的（增加了类内初始化int v = 0;），就编译过了，由此可见。。。大括号初始化容易把持不住；

#include <iostream>

class AAA {
 public:
  AAA() = default;
  int v = 0;
};

int main() {
  AAA a;
  AAA b{std::move(a)};
  return 0;
}