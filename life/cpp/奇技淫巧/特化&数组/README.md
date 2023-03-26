#include<iostream>
#include<string>
#include<sstream>
#include<map>
#include<list>
using namespace std;
template <typename T> 
struct Foo {
    static void foo(const T &t) {
		cout<<"noarr"<<endl;
    }
};
template <typename T,size_t size> 
struct Foo<T[size]> {
    static void foo(const T (&t)[size]) {
		cout<<"arr"<<endl;
    }
};
template <typename T> 
void foo(const T &t) {
     Foo<T>::foo(t);
}
int main(){
	int a = 3;
	float f[10] = {1.1,2.2,3.3,4.4};
	foo(a);
	foo(f);
	return 0;
}
