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
// [12345678901234567890123] [xxx] [xxx] [y]
// 其中第一个括号内是23个字符,x是不关注的部分，符合上述规则的就当作是日志

class Args
{
public:
    Args() {}
    bool help();
    bool ParseArgs(int argc, char* argv[]);
    std::string exclude_file;
};

bool Args::help()
{
    std::cout << "usage: ./main [-h] [-e exclude_file]" << std::endl;
    return false;
}

bool Args::ParseArgs(int argc, char* argv[])
{
    int ch; 
    while ((ch = getopt(argc, argv, "e:h")) != -1) 
    {   
        switch (ch)
        {   
        case 'e':
            exclude_file = optarg;
            break;
        case 'h':
        default:
            return help();
        }   
    }   
    return true;
}

bool IsFirstLine(const std::set<std::string>& exclude_tags, const std::string& s, const int date_size, const int ignore_bracket_after_data_size, bool& is_important) {
	std::string st = s;
	if (st.size() < date_size + 2) {
		return false;
	}
	if (st[0] != '[') {
		return false;
	}
	if (st[date_size + 1] != ']') {
		return false;
	}
	st = st.substr(date_size + 2);
	int found_br_size  = 0;
	while(true) {
		if (found_br_size >= ignore_bracket_after_data_size) {
			break;
		}
		size_t loc = st.find("[");
		if (loc == std::string::npos) {
			return false;
		}
		st = st.substr(loc + 1);
		loc = st.find("]");
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
		if (IsFirstLine(exclude_tags, st, 23, 2, is_important)) {
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
	return 0;
}
