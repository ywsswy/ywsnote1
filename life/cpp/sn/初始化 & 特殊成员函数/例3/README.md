#include <iostream>

class BBB {
 public:
  BBB() = default;
  BBB(BBB&& b) {}  // 用户声明了移动操作，所以BBB肯定不会自动生成复制操作
};

class AAA {
 public:
  AAA() = default;
  ~AAA() {}
  BBB b1;
};

int main() {
  AAA b;
  AAA c(std::move(b));
  return 0;
}

报错：
wtf.cc: In function ‘int main()’:
wtf.cc:18:21: error: use of deleted function ‘AAA::AAA(const AAA&)’
 AAA c(std::move(b));
                     ^
wtf.cc:9:7: note: ‘AAA::AAA(const AAA&)’ is implicitly deleted because the default definition would be ill-formed:
 class AAA {
       ^
wtf.cc:9:7: error: use of deleted function ‘constexpr BBB::BBB(const BBB&)’
wtf.cc:3:7: note: ‘constexpr BBB::BBB(const BBB&)’ is implicitly declared as deleted because ‘BBB’ declares a move constructor or move assignment operator

这个例子说明，如果有析构函数，则不会自动生成移动构造函数，如果真的生成了的话，不会编译报错的，但是却报错说找不到‘AAA::AAA(const AAA&)’，说明编译器尝试自动生成的是const复制构造函数；进一步，如果把析构函数注释掉就能编译通过了；