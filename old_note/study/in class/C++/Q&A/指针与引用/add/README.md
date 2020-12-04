指针也可以传引用方法传
#include<iostream>
using namespace std;
int yswap(int &a,int &b,int *&p){
	p = &a;
	return 0;
}
int main(){
	int x = 3;
	int y = 5;
	int *p = &y;
	yswap(x,y,p);
	return 0;
}
//多运算符时，根据结合律看最近的那个
首先p变量是个引用，其次，这个引用的真实对象是个int型指针
引用必须被初始化
C++中引用是编译器通过指针实现的，但这个实现在语言层面对程序员做了透明化处理。
