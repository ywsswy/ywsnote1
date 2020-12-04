#include <iostream>  
using namespace std;  
class A  
{  
private:  
    int n;  
public:  
    A(int m):n(m)  
    { }  
    ~A(){}  
};  
int main()  
{  
    A a(1);  //栈中分配  
    A b = A(1);  //栈中分配  
    A* c = new A(1);  //堆中分配  
　　delete c;  
    return 0;  
}  
