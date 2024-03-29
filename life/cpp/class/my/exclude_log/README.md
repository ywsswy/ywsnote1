#include <unistd.h>
#include <fstream>
#include <iostream>
#include <vector>
#include <string>
#include <set>

// 把日志中无关紧要的垃圾部分过滤掉
// |grep -v 做不到，因为有些日志是多行打印的
// 注意：仅当日志特别快的时候，每秒钟都在输出的时候，才有必要使用本工具

// 日志的格式要求：
// [2022-10-28 18:37:54.485] [thread 383739] [error] [external/trpc_cpp/trpc/naming/polaris/polaris_selector.cc:316]
// 1234567890123456789012345 xxx xxx xxx yyy
// C++ 格式
// 其中前25个字符长度固定，然后看忽略多少个间隔符号，可以是空格，可以是tab，x是不关注的部分，符合上述规则的就当作是日志
// 2022-12-09 21:50:14.044  ERROR onepiece/policy.go:24 req policy service failed YWS force err
// 12345678901234567890123\txxx\tyyy
// go 格式

class Args {
 public:
  Args() {}
  bool help();
  bool ParseArgs(int argc, char* argv[]);
  std::string exclude_file;
  std::string log_type;  // "c++"(default), "go"
};

bool Args::help() {
    std::cout << "usage: ./main [-h] [-e exclude_file]" << std::endl;
    return false;
}

bool Args::ParseArgs(int argc, char* argv[]) {
    int ch; 
    while ((ch = getopt(argc, argv, "e:t:h")) != -1) {
        switch (ch) {
        case 'e':
            exclude_file = optarg;
            break;
        case 't':
            log_type = optarg;
            break;
        case 'h':
        default:
            return help();
        }
    }
    if (log_type != "c++" && log_type != "go") {
      log_type = "c++";
    }
    return true;
}

// ignore_bracket_after_data_size: 忽略多少对括号（c++）或者忽略多少个tab（go）

bool IsFirstLine(const std::set<std::string>& exclude_tags, const std::string& s, const int date_size, const std::string& ignore_sign, const int ignore_count, bool& is_important) {
  std::string st = s;
  if (st.size() < date_size) {
    return false;
  }
  st = st.substr(date_size);
  int found_br_size = 0;
  while (true) {
    if (found_br_size >= ignore_count) {
      break;
    }
    size_t loc = st.find(ignore_sign);
    if (loc == std::string::npos) {
      return false;
    }
    st = st.substr(loc + 1);
    ++found_br_size;
  }
  is_important = true;
  for (const auto& tag: exclude_tags) {
    if (tag.empty()) {
      continue;
    }
    if (st.find(tag) == 0) {
      is_important = false;
    }
  }
  return true;
}

int main(int argc, char* argv[]) {
  Args info;
  if (!info.ParseArgs(argc, argv)) {   
    exit(-1);
  }
  std::ifstream in(info.exclude_file, std::ios::in);
  if (!in.is_open()) {
    exit(-1);
  }
  std::string st;
  std::set<std::string> exclude_tags;
  // 加载需要排除掉的垃圾日志信息
  // 即符合什么特征的日志要丢掉
  while(getline(in, st)) {
    exclude_tags.insert(st);
  }
  std::vector<std::string> saved_logs;
  bool last_is_important = false;
  while(getline(std::cin, st)) {
    // 判断st是否是日志首行
    bool is_important = true;
    if (IsFirstLine(exclude_tags, st, (info.log_type == "c++" ? 25 : 23), (info.log_type == "c++" ? " " : "\t"), (info.log_type == "c++" ? 4 : 2), is_important)) {
      // 如果是首行，判断之前的是否需要输出
      if (last_is_important && saved_logs.size() > 0) {
        for (const auto& saved_log : saved_logs) {
          std::cout << saved_log << std::endl;
        }
      }
      last_is_important = is_important;
      saved_logs.clear();
    }
    // 否则不做任何处理
    saved_logs.emplace_back(st);
  }
  if (last_is_important && saved_logs.size() > 0) {
    for (const auto& saved_log : saved_logs) {
      std::cout << saved_log << std::endl;
    }
  }
  return 0;
}
