// std::for_each (since C++17)
vector<struct> to string，怎么传入自定义的转换函数呢，哪种性能最高最通用
```
#include <vector>
#include <string>

template<typename T>
std::string VectorToString(const std::vector<T>& vec, std::string (*elementToString)(const T&)) {
    std::string result;
    for (const auto& elem : vec) {
        result += elementToString(elem);
    }
    return result;
}

#include <vector>
#include <string>
#include <numeric>

template<typename T>
std::string VectorToString(const std::vector<T>& vec, std::string (*elementToString)(const T&)) {
    return std::accumulate(vec.begin(), vec.end(), std::string(),
                           [&](std::string s, const T& e) { return s + elementToString(e); });
}


#include <iostream>
#include <string>
#include <vector>
#include <numeric>

std::string IntToString(const int i) {
  return std::to_string(i);
}

int main() {
  std::vector<int> vec;
  vec.push_back(1);
  vec.push_back(3);
  vec.push_back(2);

  std::string s = std::accumulate(vec.begin(), vec.end(), std::string(), [&](std::string s, const int& e) { return s + IntToString(e); });
  std::cout << s << std::endl;
  return 0;
}


#include <vector>
#include <string>
#include <sstream>

template <typename T>
std::string VectorToString(const std::vector<T>& vec, std::string (*elementToString)(const T&)) {
    std::ostringstream oss;
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        if (it != vec.begin()) {  // 如果不是第一个元素，添加逗号
            oss << ",";
        }
        oss << elementToString(*it);
    }
    return oss.str();
}



#include <vector>
#include <string>
#include <algorithm>

template <typename T>
std::string VectorToString(const std::vector<T>& vec, std::string (*elementToString)(const T&)) {
    std::string result;
    bool first = true;
    std::for_each(vec.begin(), vec.end(),
                  [&](const T& elem) {
                      if (!first) {  // 如果不是第一个元素，添加逗号
                          result += ",";
                      } else {
                          first = false;
                      }
                      result += elementToString(elem);
                  });
    return result;
}
```

```
#include <vector>
#include <string>
#include <sstream>
#include <iterator>
#include <iostream>

int main()
{
  int* p = nullptr;
  delete p;
  std::vector<int> vec;
  vec.push_back(1);
  vec.push_back(4);
  vec.push_back(7);
  vec.push_back(4);
  vec.push_back(9);
  vec.push_back(7);

  std::ostringstream oss;

  if (!vec.empty())
  {
    // Convert all but the last element to avoid a trailing","
    std::copy(vec.begin(), vec.end()-1,
        std::ostream_iterator<int>(oss,","));

    // Now add the last element with no delimiter
    oss << vec.back();
  }

  std::cout << oss.str() << std::endl;
}
```