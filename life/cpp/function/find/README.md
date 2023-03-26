// since c++11
// 如果是二分查找则使用std::binary_search 和 std::lower_bound
```
#include <algorithm>
#include <iostream>
#include <vector>
int main() {
  std::vector<int> ve;
  ve.push_back(12);
  ve.push_back(1);
  ve.push_back(2);
  int find_value = 22;
  // 单纯比较==
  auto&& iter = std::find(ve.begin(), ve.end(), find_value);  
  if (iter != ve.end()) {
    std::cout << "find:" << *iter << std::endl;
  } else {
    std::cout << "not find" << std::endl;
  }
  // 有自定义处理的==
  if (ve.end() == std::find_if(ve.begin(), ve.end(), [&](auto&& item) { return item == find_value; })) {
    std::cout << "not find" << std::endl;
  }
  // 什么时候用std::findxx什么时候用普通的for呢，如果需要得到一个是否找到的flag标志的话，就用std::findxx，其他不关心是否找到的情况都用普通的for
}
```