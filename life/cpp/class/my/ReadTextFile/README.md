
bool ReadTextFile(const std::string &filename, std::vector<std::string> &data,
                  const int skipline = 0) {
  std::ifstream ifs(filename.c_str());
  if (!ifs.is_open()) {
    std::cerr << "file open failed, " << filename << std::endl;
    return false;
  } else {
    std::string line_data;
    int linenum = 0;
    while (getline(ifs, line_data)) {
      if (++linenum > skipline) {
        data.push_back(line_data);
      }
    }
    ifs.close();
    return true;
  }
}