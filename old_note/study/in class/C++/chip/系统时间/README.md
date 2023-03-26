关于获取系统时间问题
若要精确到毫秒级，windows平台下可以使用如下代码：
SYSTEMTIME yst;
::GetLocalTime(&yst);
fprintf(file, "%04d-%02d-%02d %02d:%02d:%02d.%03d",\
    yst.wYear, yst.wMonth, yst.wDay,\
    yst.wHour, yst.wMinute, yst.wSecond, yst.wMilliseconds);
若要精确到毫秒级，兼容windows和linux平台可以在普通获取秒时间的基础上加入如下代码获取毫秒部分：
#include <iostream>
#include <sys/timeb.h>
struct timeb tb;
ftime(&tb);
std::cout << tb.millitm;

