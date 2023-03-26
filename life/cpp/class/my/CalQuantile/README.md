#include <iostream>
#include <vector>
#include <algorithm>

template <typename ItemType>
void CalQuantile(const std::string& title, const std::vector<ItemType>& c_data,
                 const std::vector<double>& quantile) {
  std::cout << "[" << title << "]" << std::endl;
  if (c_data.size() == 0) {
    std::cout << "data empty" << std::endl;
    return;
  }
  std::vector<ItemType> data = c_data;
  std::sort(data.begin(), data.end());
  double sum = 0;
  for (auto&& i : data) {
    sum += i;
  }
  std::cout << "count:" << data.size() << std::endl;
  std::cout << "sum:" << sum << std::endl;
  std::cout << "avg:" << sum / data.size() << std::endl;
  std::cout << "min:" << data[0] << std::endl;
  std::cout << "max:" << data[data.size() - 1] << std::endl;
  std::vector<double> quantile_data;
  for (auto&& i : quantile) {
    double real_loc = (1 + data.size()) * i / 100;
    size_t loc = static_cast<size_t>(real_loc);
    double cast_loc = static_cast<double>(loc);
    if (cast_loc == real_loc) {
      quantile_data.push_back(data[loc - 1]);
    } else {
      size_t left_loc = 0;
      size_t right_loc = 0;
      if (cast_loc > real_loc) {
        right_loc = loc;
        left_loc = loc - 1;
      } else {
        left_loc = loc;
        right_loc = loc + 1;
      }
      if (left_loc == 0) {
        quantile_data.push_back(data[0]);
      } else if (right_loc - 1 >= data.size()) {
        quantile_data.push_back(data[data.size() - 1]);
      } else {
        quantile_data.push_back(data[left_loc - 1] +
                                (data[right_loc - 1] - data[left_loc - 1]) *
                                    (real_loc - static_cast<double>(left_loc)));
      }
    }
  }
  size_t idx = 0;
  for (auto&& i : quantile) {
    std::cout << "P" << i << ":" << quantile_data[idx] << std::endl;
    ++idx;
  }
}


int main() {
  std::vector<int> bl_num;
  bl_num.push_back(11);
  bl_num.push_back(17);
  bl_num.push_back(19);
  bl_num.push_back(3);
  bl_num.push_back(5);
  bl_num.push_back(9);
  std::vector<double> quantile;
  quantile.push_back(80);
  quantile.push_back(90);
  quantile.push_back(95);
  quantile.push_back(99);
  CalQuantile("bl_num", bl_num, quantile);
  return 0;
}