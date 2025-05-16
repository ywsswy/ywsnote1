emplace  // 已存在的key是无法再插入的


std::map<std::pair<int, int>, int> ret 没有错
但是std::unordered_map<std::pair<int, int>, int> 有错，因为前者需要的是std::pair<int, int>的比较函数（能默认生成），而后者需要std::pair<int, int>的hash函数，不能默认生成