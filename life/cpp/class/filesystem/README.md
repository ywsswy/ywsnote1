 std::filesystem::directory_entry entry_tmp_file{"/data/tmp"};
  if (!entry_tmp_file.exists()) {
    std::cout << "not exist" << std::endl;
    return -1;
  }
  if (entry_tmp_file.is_directory()) {
    std::cout << "is dir" << std::endl;
    std::filesystem::directory_iterator d_begin{FLAGS_input};
    std::filesystem::directory_iterator d_end;

    while (d_begin != d_end) {
      auto dstDirName = d_begin->path().filename();
      if (dstDirName != "." && dstDirName != "..") {
        std::cout << dstDirName.string() << std::endl;
      }
      ++d_begin;
    }
  } else {
    std::cout << "is file" << std::endl;
  }