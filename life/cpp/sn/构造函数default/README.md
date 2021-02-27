
class A
{
public:
A(int B) {
};
};


A a; //如果定义一个对象时没有使用初始化式，这种编译报错，因为，只要自定义了构造函数，编译器就不会自动生成默认构造函数了，这种




A() = default;效果上和自定义一个A() { b =0;}相同？？？



类似的骚操作有delete，可以禁止一个函数被调用，例如禁止拷贝禁止赋值，比private更狠

Test(const Test&) = delete;
Test& operator = (const Test&) = delete;