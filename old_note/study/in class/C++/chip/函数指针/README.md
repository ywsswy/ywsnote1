#include <stdio.h>
#include <malloc.h>
#include <stdlib.h>
//函数名本身是一个指针
//处理无同、参数无异的基本函数元
int f1(int a,int b){
	return a+b;
}
int f2(int a,int b){
	return a-b;
}
int f3(int a,int b){
	return a*b;
}
//调用基本函数元的处理函数
void yF(int a,int b,int (*fn)(int,int)){
	printf("%d\n",(*fn)(a,b));
}
int main(){
	int a = 4;
	int b = 3;
	//归纳出基本函数指针类型
	int (*fn)(int,int) = NULL;
	//函数指针指向不同函数，进行调用
	fn = f1;
	yF(a,b,fn);
	fn = f2;
	yF(a,b,fn);
	fn = f3;
	printf("%d\n",fn(a,b));
	return 0;
}
