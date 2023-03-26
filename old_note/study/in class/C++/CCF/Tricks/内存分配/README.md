静态存储区
全局数据区
局部变量 栈区
动态分配 堆区
（new出来的，地址可以作为返回值返回）
【
数组的总大小不得超过 0x7fffffff bytes
全局 	0x70000000 bytes
局部 	0x80000 （200M)
动态堆 	0x20000000
【
局部变量在使用前必须赋值，否则值未定
全局变量在使用前没赋值的话为默认值：
数字 默认值都为0
bool false
string 空字符串
char '\0'
【memset
bool va[100][100];
memset(va,value,sizeof(va));
//按字节大片赋值，所以，value取值只有0~0xff有效，所以通常只取0或-1
