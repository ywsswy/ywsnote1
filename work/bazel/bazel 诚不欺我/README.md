xxx_test.cc:13:10: fatal error: yyy.h: No such file or directory
 #include "yyy.h"

include写完整relative路径


所在BUILD里要写deps = [
  "//relative/to/path:<cc_library name|cc_test name>",

并且保证<cc_test> 的srcs里有该文件