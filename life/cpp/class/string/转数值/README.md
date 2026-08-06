## c++  <string>
输入参数是std::string

std::stoi  // int
std::stol
std::stoll  // long long
std::stoul
std::stoull  // unsigned long long
std::stod  // double


## c  <cstdlib>（不会抛异常）
输入参数是char*

简化版（只支持十进制）
std::atoi
std::atol
std::atoll
std::atof  // double


加强版（可以指定base）
std::strtol
std::strtoll
std::strtoull
std::strtod  // double
