https://zhuanlan.zhihu.com/p/56355184

// 导出类的方法
#include <boost/python.hpp> 
using namespace boost::python; 
  
class Test 
{ 
public: 
 int Add(const int x, const int y)  
 { 
  return x + y;  
 } 
  
 int Del(const int x, const int y)  
 { 
  return x - y;  
 } 
}; 
  
BOOST_PYTHON_MODULE(test3) 
{ 
 class_<Test>("Test") 
  .def("Add", &Test::Add) 
  .def("Del", &Test::Del); 
}


// 关键点：几处必须相同-o test3
g++ test3.cpp -std=c++11 -fPIC -shared -o test3.so -I/usr/include/python3.6m -I/usr/local/include/boost -L/usr/lib64 -lboost_python3 


// python2
import test3
test=test3.Test()
test.Add(1,2)