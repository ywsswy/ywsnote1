string s1;
s1[0] = 'y';

string有类似写时复制（copy-on-write）的优化，就是复制赋值的时候可能并不是立即分配相同大小的字符数组，而是智能指针复制，等到修改字符串内容的时候，才真的new出一块进行修改；


const char* kKey = "key"; std::string s(kKey);  // size = 3,, sizeof(kKey) = 8（因为是指针。。） 不存在特殊字符的初始化
const char kKey[] = "key"; std::string s(kKey);  // size = 3, sizeof(kKey) = 4
const char kKey[] = {'k', '\x00', 'e', 'y'}; std::string s(kKey, sizeof(kKey));  // size = 4, sizeof(kKey) = 4，存在特殊字符的初始化（用十六进制表示），注意使用{}来初始化字符数组的话，结尾就不会填'\00'，就是sizeof不会多一个字节；另外std::cout << s时是能成功输出\x00字符的


raw string（原生字符串）可以不加转义的初始化字符串：
R"[<tokens>](<字符串>)[<tokens>]"; R"(say:"hi")" 即 say:"hi"

[member
std::string::npos 代表字符串没找到的size_t -1 && 最大值

[methed
std::string(reinterpret_cast<char *>(&value), 8);  // 不写size，则是拷贝（总之新分配内存）到空字符为止，NULL会core_dump!!
append	//string& append(string) 同 s = s+t
back	//引用
front	//引用
find	//size_t find(s_string,startloc_sizet)	//find s start at object[startloc]
rfind	//size_t rfind(s_string,startloc_int)	//同上.. (从后往前，next startloc_int-1)
	//	"1234".rfind("23",1)是可以找到的
find_first_not_of	//size_t find_first_not_of(s_string,startloc_int)	//找到跟s中任一字符都不匹配的位置
find_first_of
find_last_not_of
find_last_of            //"1234".find_last_of("2",1)是可以找到的，第二个参数x表示只找[0,x]范围内的位置
replace	//1)string& replace(startloc_int,len_int,s)	//0s startloc要保证<=size，不然会core_dump
	//2)string& replace(start_ite,end_ite,s)	//end_ite will not be erased
substr	//string substr(startloc_int,len_int=max)       //startloc要保证<=size，不然会core_dump
erase  // iterator erase(iterator first, iterator last)  // 原地操作remove [first,end)的元素

[associated function
string to_string(T)	//C++11
getline(std::cin,st);	//读到的换行符丢弃，回车符不会丢弃（写到接收者中，只不过有的操作系统可能本身就没有回车符）
cin或者ifstream的读取;//正常读整数/字符串之类时，读取会忽略"开始"的空格换行符回车符，再遇空格换行符回车符会停止读取，空格回车符换行符都不会写到接收者中

[others
+ 	连接运算符
<>	比较运算符：ascii大，无相当于0