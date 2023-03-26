#include <iostream>
#include <string>
#include "YLog.h"
int main(){
	Log log1(Log::INFO,"log1.txt",Log::ADD);
	int a = 520;
	double b = 13.14;
	std::string c = "I love U.";
	log1.w(__FILE__, __LINE__, Log::INFO, "watch_a",a);
	log1.w(__FILE__, __LINE__, Log::INFO, "Watch_b",b);
	log1.w(__FILE__, __LINE__, Log::INFO, "watch_c",c);
	return 0;
}
/*
# 可以继续考虑的部分：
1）avoid large file.
即轮替，作为服务器日志，服务器不出故障是不重启的，半年一年的日志放到一个文件会导致文件过大。
# 根据不同需求可添加如下功能：
1）判断输出日志的路径文件夹是否存在，不存在则创建文件夹，代码如下：
*/

