string s1;
s1[0] = 'y';

[member
string::npos 代表字符串没找到的size_t -1 && 最大值

[methed
std::string((const char *)&value, 8);  // std::string(const char *(&value), 8))这种就报错
append	//string& append(string) 同 s = s+t
back	//引用
front	//引用
find	//size_t find(s_string,startloc_sizet)	//find s start at object[startloc]
rfind	//size_t rfind(s_string,startloc_int)	//同上.. (从后往前，next startloc_int-1)
	//	"1234".rfind("23",1)是可以找到的
find_first_not_of	//size_t find_first_not_of(s_string,startloc_int)	//找到s中任何字符都不匹配的位置（找首个非法字符）
find_first_of
find_last_not_of
find_last_of            //"1234".find_last_of("2",1)是可以找到的
replace	//1)string& replace(startloc_int,len_int,s)	//0s startloc要保证<=size，不然会core_dump
	//2)string& replace(start_ite,end_ite,s)	//end_ite will not be erased
substr	//string substr(startloc_int,len_int=max)       //startloc要保证<=size，不然会core_dump

[associated function
string to_string(T)	//C++11
getline(cin,st);	//只有此行（有字符或者回车）才会读成功(if(getline))，getline不会忽略空格和换行，但是独到的换行不会保存，仅当作此次读取完毕。
cin;//正常读整数/字符串之类时，开始读取会忽略空格换行，再遇空格换行会停止读取

[others
+ 	连接运算符
<>	比较运算符：ascii大，无相当于0