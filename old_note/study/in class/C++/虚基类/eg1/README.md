#include <iostream>
using namespace std;
class BaseO {
public:
BaseO (int var) : varO (var) {}
int varO;
void funO () {cout<< "Member of BaseO"<< endl; }
class Basel: virtual publ BaseO {
public:
Basel (int var) : BaseO (var) {}
int varl;
}
class Base2: virtual public BaseO {
public:
Base2 (int var) : BaseO (var) {}
int var2;
) ;
class Derived: public Basel, public Base2 {
public:
Der ved(int var) : BaseO(var) , Basel (var) , Base2 (var) {}
nt vari
roid fun () {cout<< "Member of Deri ved"<< endl; }
int main () {
Der ved d(l);
d.var=2;
d.fun();
return 0; 
｝
