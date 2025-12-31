#include<iostream>
#include<queue>
#include<vector>
using namespace std;

class YT{
 public:
  YT(double a, int b){ v = b; }
  int v = 0;
  std::string xx;
};

struct CompareByFirst {
  constexpr bool operator()(YT const& a, YT const& b) const noexcept {
    return a.v < b.v;
  }
};
 
int main() {
        priority_queue<YT, std::vector<YT>, CompareByFirst> p;
        p.push(YT(1,1));
        p.push(YT(1,2));
        p.push(YT(1,3));
 
        while (!p.empty())  // 输出 3 2 1，说明默认常识是大根堆，如果operator<是反常识实现就是小根堆；
        {
                cout << p.top().v << " ";
                p.pop();
        }
        cout << endl;
 
 
        return 0;
}