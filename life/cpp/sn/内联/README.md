一个函数可以同时用inline 和 virtual修饰，但是是否真的进行了内联要视情况而定；
内联是在编译期建议编译器内联的，所以是否真的进行了内联取决于编译期能否去确定其运行期调用哪个代码；

## 判断A函数调用B函数时B函数是否被内联：
- 首先直接分析最后的可执行文件可能比较大不好分析，
- 所以可以直接分析主调方（A文件中调用B函数，则分析A）的relocatable文件，看A的主调函数的汇编（用objdump）代码中，是否存在callq B函数的指令；
- 但是在relocatable文件中，所有的函数调用指令都是没有写地址的（见objdump），所以可能会出现汇编代码量非常大不方便锁定主调函数的位置时，一堆callq指令不确定里面有没有callq B的，需要结合objdump -r一起分析（看主调函数范围内的重定向信息中是否含有B函数的符号）；
- 另外如果整个relocatable文件中都没有B函数符号的话，那B毫无疑问肯定被内联了
- 另外考虑强制内联使用：`__attribute__((always_inline)) inline `，反之则有__attribute__((noinline))，默认构造函数也是可以指定的；





## 下面的例子我不确定是否正确，需要通过objdump -d xxx.o来验证一下
```
#include <iostream>
class Base{
 public:
    inline virtual void who(){
      std::cout << "Base" << std::endl;
    }
};

class Derived : public Base {
 public:
  inline virtual void who() override {
    std::cout << "HELLO" << std::endl;
  }
};


void f2() {
 Base b;
 b.who();  // 此处的虚函数 who()，是通过类（Base）的具体对象（b）来调用的，编译期间就能确定了，所以它可以是内联的，但最终是否内联取决于编译器。

 Base *bptr = new Derived();  // 此处的虚函数是通过指针调用的，呈现多态性，需要在运行时期间才能确定，所以不能为内联。
 bptr->who();
}

int main(){
  f2();
 return 0;	
}

```