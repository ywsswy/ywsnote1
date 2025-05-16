// 如果代码直接写std::vector<std::string> v = {"a","b"......};或者写成 ve.push_back("a");ve.push_back("b");......很多的时候，编译器会耗时长或者搞不定，，这时候可以std::string s = "a b c..."，然后结合下面这个函数来高性能编译

std::vector<std::string> SplitString(const std::string& raw, const std::string& inter, const bool ignore_empty) {
  std::vector<std::string> vec;
  size_t n = 0;
  size_t old = 0;
  while (n != std::string::npos) {
    n = raw.find(inter, n);
    if (n != std::string::npos) {
      if (!ignore_empty || n != old) {
        vec.push_back(raw.substr(old, n - old));
      }
      n += inter.size();
      old = n;
    }
  }
  if (!ignore_empty || old < raw.size()) {
    vec.push_back(raw.substr(old, raw.size() - old));
  }
  return vec;
}