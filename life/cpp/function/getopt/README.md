示例见class/my/Args

int getopt(int argc, char * const argv[], const char *optstring); 


//只能解析短选项: -d 100,不能解析长选项：--prefix


char*optstring = “ab:c::”;
单个字符a         表示选项a没有参数            格式：-a即可，不加参数
单字符加冒号b:     表示选项b有且必须加参数      格式：-b 100或-b100,但-b=100错
单字符加2冒号c::   表示选项c可以有，也可以无     格式：-c200，其它格式错误

getopt 函数将依次检查命令行是否指定了 -a， -b， -c(这需要多次调用 getopt 函数，直到其返回-1)，当检查到上面某一个参数被指定时，函数会返回被指定的参数名称(即该字母)
optarg —— 指向当前选项参数(如果有)的指针