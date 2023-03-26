```
class AAA {
 public:
  AAA() {
    v1 = 6;
  }
  int v1{4};
  int v2{5};
};
void Fun() {
  AAA a;
}
这段函数则不会在Fun函数栈内初始化a，而是生成了_ZN3AAAC1Ev符号，用函数调用的方式进行初始化，构造函数内部先初始化成员变量的默认值设置，然后再执行用户自定义的语句块；而且就算自定义成`AAA() {}`也会生成_ZN3AAAC1Ev符号的，所以不要以为默认构造函数里啥都没写就不影响性能；
但是main.o中有：
0000000000000000 W _ZN3AAAC1Ev（在.text._ZN3AAAC2Ev section开头）
0000000000000000 W _ZN3AAAC2Ev（是.text._ZN3AAAC2Ev section的名称后缀，用于 "placement new" 的构造函数，因为这里和_ZN3AAAC1Ev的实现相同，所以编译器将其作为前者的别名，属于减少重复代码的优化）
```