#include <functional>

struct YLogForC
{
  void InitYLog_(int x){}
  
};


int main()
{
  YLogForC o1;
  std::function<void (int)> f1 = std::bind(&YLogForC::InitYLog_, o1, std::placeholders::_1);
}