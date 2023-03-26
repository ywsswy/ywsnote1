不回显得获取密码
#include <unistd.h>

char* password = getpass("Input password:");
pw = std::string(password);
delete password;