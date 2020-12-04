#include <iostream>
using namespace std;// 8-3 包含虚成员函数的 Basel 类及派
class Base1{ //基类 Basel 定义
public:
	virtual void display() const;//试着看没有virtual会怎样
};
void Base1::display() const{
	cout<< "Base1::display() "<< endl;
}
class Base2: public Base1{
public:
	void display () const;//派生类覆盖基类的成员函数时，既可以使用 virtual 关键字，也可以不使用
};
void Base2::display () const {
	cout<< "Base2::display () "<< endl;
}
class Derived: public Base2{
public:
	void display () const;
};
void Derived::display() const{
	cout<< "Derived::display() "<<endl;
}
void fun(Base1 *ptr){
	ptr->display();//ptr 一>Basel:: display();可以强制调用被覆盖的基类
}
int main(){
	Base1 base1;
	Base2 base2;
	Derived derived;
	fun(&base1);
	fun(&base2);
	fun(&derived);
	return 0;
}
//在本程序中，派生类并没有显式给出虚函数声明，这时系统就会遵循以下规则来判断
派生类的一个函数成员是不是虚函数。
·该函数是否与基类的虚函数有相同的名称。
·该函数是否与基类的虚函数有相同的参数个数及相同的对应参数类型。
·该函数是否与基类的虚函数有相同的返回值或者满足赋值兼容规则的指针、引用
型的返回值。
如果从名称、参数及返回值 个方面检查之后，派生类的函数满足了上述条件，就会
自动确定为虚函数。这时，派生类的虚函数便覆盖了基类的虚函数。不仅如此，派生类巾
的虚函数还会隐藏基类中同名函数的所有其他重载形式。
