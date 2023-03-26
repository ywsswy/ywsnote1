//boost的查看类型推导更准确
#include <boost/type_index.hpp>
#include <iostream>
using boost::typeindex::type_id_with_cvr;
template<typename T>
void f1(const T& param)
{
    std::cout << type_id_with_cvr<T>().pretty_name() << std::endl;
    std::cout << type_id_with_cvr<decltype(param)>().pretty_name() << std::endl;
}
