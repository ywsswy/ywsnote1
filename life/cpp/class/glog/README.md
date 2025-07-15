#include <glog/logging.h>

google::InitGoogleLogging(argv[0]);

FLAGS_logbufsecs = 0;  // 设置日志实时刷新，不写缓冲区

