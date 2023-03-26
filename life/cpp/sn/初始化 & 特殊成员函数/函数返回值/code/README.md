#include <iostream>
#include <string>
#include <memory>
class BBB{
 public:
  int a = 1;
  int b = 2;
  int c = 3;
};

class AAA{
 public:
  AAA(int a) {
    this->a = a;
    std::cout << "AAA(" << this->a << ")" << std::endl;
    s = "fawjefoiajwoefjwe___" + std::to_string(a);
  }
  ~AAA() {
    std::cout << "~AAA(" << this->a << ")" << std::endl;
  }
  AAA(const AAA& a) {
    std::cout << "AAA(const &)" << std::endl;
  }
  AAA& operator=(const AAA& a) {
    std::cout << "=AAA(const &)" << std::endl;
  }
  AAA(AAA&& a) {
    std::cout << "AAA(&&)" << std::endl;
  }
  AAA(const AAA&& a) {
    std::cout << "AAA(const &&)" << std::endl;
  }
  AAA& operator=(AAA&& a) {
    std::cout << "=AAA(&&" << this->a << "<-" << a.a << ")" << std::endl;
  }
  AAA& operator=(const AAA&& a) {
    std::cout << "=AAA(const &&)" << std::endl;
  }
  int a = 0;
  BBB b;
  int c = 4;
  int d = 5;
  int e = 6;
  std::string s;
};

AAA Fun2() {
  std::cout << "FWIOAEFJOIAWJFOAWEJFFE" << std::endl;
  AAA b(2);
  b.e = 7;
  std::cout << "FWIOAEFJOIAWJFOAWEJFFE" << std::endl;
  return b;
}

void Fun() {
  AAA a(1);
  a.e = 7;
  a = Fun2();
}

int main() {
  Fun();
  return 0;
}