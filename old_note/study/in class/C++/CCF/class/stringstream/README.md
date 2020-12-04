#include<sstream>
string s = "17";
stringstream ss;
ss<<s;
int i;
ss>>i;//通过stringstream将string转为int
每次转换完要再次使用需要ss.clear()
不支持stringstream ss(10)这种构造函数
#############################################################
用std::cin 读了 int，std::string之后，本行的回车是不会被读走的。
因此如果紧接着使用getline就会被getline读到一个空串。所以用如下清空此垃圾行
{
	std::string trash = "";
	getline(std::cin,trash);
}
此外如果stringstream中是"123 456",>>int/string后是" 456"，如果是"123",>>int/string后依然是"123"，所以不要太相信stringstream，多用find_方法判断（参考201903-4）
ss << "123 456"是全部放入流，ss >> string是输出123
