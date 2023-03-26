//运行时获取

#include <boost/type_index.hpp>
#include <iostream>
template<typename T>
class TD
{
public:
    TD()
    {
        name_ = boost::typeindex::type_id_with_cvr<T>().pretty_name();
    }
    const std::string &name()
    {
        return name_;
    }
private:
    std::string name_;
};

class Base {};
class Derived: public Base {};

int main()
{
    Base b, *pb;
    pb = NULL;
    Derived d;

    std::cout << typeid(int).name() << '\t'
        << TD<int>().name() << std::endl

        << typeid(unsigned).name() << '\t'
        << TD<unsigned>().name() << std::endl

        << typeid(long).name() << '\t'
        << TD<long>().name() << std::endl

        << typeid(unsigned long).name() << '\t'
        << TD<unsigned long>().name() << std::endl

        << typeid(char).name() << '\t'
        << TD<char>().name() << std::endl

        << typeid(unsigned char).name() << '\t'
        << TD<unsigned char>().name() << std::endl

        << typeid(float).name() << '\t'
        << TD<float>().name() << std::endl

        << typeid(double).name() << '\t'
        << TD<double>().name() << std::endl

        << typeid(std::string).name() << '\t'
        << TD<std::string>().name() << std::endl

        << typeid(Base).name() << '\t'
        << TD<Base>().name() << std::endl

        << typeid(b).name() << '\t'
        << TD<decltype(b)>().name() << std::endl

        << typeid(pb).name() << '\t'
        << TD<decltype(pb)>().name() << std::endl

        << typeid(Derived).name() << '\t'
        << TD<Derived>().name() << std::endl

        << typeid(d).name()<< '\t'
        << TD<decltype(d)>().name() << std::endl;
    return 0;
}


```
结果

i       int
j       unsigned int
l       long
m       unsigned long
c       char
h       unsigned char
f       float
d       double
Ss      std::string
4Base   Base
4Base   Base
P4Base  Base*
7Derived        Derived
7Derived        Derived
```