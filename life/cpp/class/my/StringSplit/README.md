std::vector<std::string> SplitString(const std::string& raw,
                                     const std::string& inter,
                                     const bool ignore_empty) {
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