#include <unistd.h>

std::string get_mem() {
  int pid = getpid();  // 获取当前进程id
  std::string file_name = "/proc/" + std::to_string(pid) + "/statm";
  std::ifstream in(file_name, std::ios::in);
  std::string res; 
  getline(in, res);  // 获取占用内存情况
  return res;
}