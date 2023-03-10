#include <unistd.h>
#include <iostream>

class Args {
 public:
  Args() {}
  bool Help();
  bool ParseArgs(int argc, char* argv[]);
  std::string input;
  std::string type;
};

bool Args::Help() {
  std::cout << "usage: ./main [-h] [-t type] [-i input]" << std::endl;
  return false;
}

bool Args::ParseArgs(int argc, char* argv[]) {
  int ch;
  while ((ch = getopt(argc, argv, "t:i:h")) != -1) {
    switch (ch) {
      case 't':
        type = optarg;
        break;
      case 'i':
        input = optarg;
        break;
      case 'h':
      default:
        return Help();
    }
  }
  if (type != "en" && type != "de") {
    return Help();
  }
  return true;
}

int main(int argc, char* argv[]) {
    Args info;
    if (!info.ParseArgs(argc, argv)) {   
        exit(-1);
    }   
    return 0;
}