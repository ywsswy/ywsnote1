/////////////////////////////////
#include<fstream> 
ofstream log("D:\\tt\\out1.txt");
streambuf * oldbuf = cout.rdbuf(log.rdbuf());
