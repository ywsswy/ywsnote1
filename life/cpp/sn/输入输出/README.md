【速度】###########################################
不用scanf printf cstdio
坚守cout cin
方法就是：cin.tie(0)，并且，所有的endl改为'\n'
##################################################
【重定向
//考虑复原
std::ifstream if1("yin1.txt");
std::streambuf* oldbuf = std::cin.rdbuf(if1.rdbuf());
std::cin.rdbuf(oldbuf);//复原
//不考虑复原
std::cin.rdbuf((new std::ifstream("yin1.txt"))->rdbuf());
std::cout.rdbuf((new std::ofstream("yout1.txt"))->rdbuf());


【推荐用条件状态
操作符：返回流对象，所以可以插在输出数据中
<iostream>
[eg:1
精确到小数点后两位
std::cout.setf(std::ios::fixed);
std::cout.precision(2);
std::cout << result << '\n';
使用后需要复原的话
std::cout.unsetf(std::ios::fixed);
std::cout.precision(6);
std::cout << result << '\n';


left
right		//【默认？ 右对齐（在左侧补充填充字符）
internal	//符号位和数字之间填充字符

boolalpha	//1或0 改为显示 true或false
noboolalpha	//【默认
hex		//16进制（不带前缀小写），16进制读取 std::cin >> std::hex >> number1 >> number2
oct		//八进制显示整数（不带前缀）
dec		//【默认/十进制
showbase	//带前缀
noshowbase	//【默认
uppercase	//大写0X E
nouppercase	//【默认
showpos		//非负数显示+
noshowpos	//【默认
fixed		//浮点数为定点十进制

//用不到
scientific	//【默认？浮点数为科学计数法
		//【默认 如果写成科学计数法是指数<-4或者大于等于精度时会自动转换为科学计数法
endl		//插入行分隔符，刷新ostream缓冲区
ends		//插入空字符（NULL）
flush		//刷新ostream缓冲区
unitbuf		//每次输出都刷新缓冲区
nounitbuf	//【默认
skipws		//【默认输入跳过空白符
noskipws
//未支持
hexfloat
defaultfloat
<iomanip>
setw		//只此仅对下一个有效
setfill
setprecesion
setbase		//设置整数输出进制



# example：

std::cout << std::hex;
std::cout << <var>; # 需要是int的，char不行，而且如果没有unset的话会一直保持