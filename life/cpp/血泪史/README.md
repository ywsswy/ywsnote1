reserve就要用push_back
输出处理放在循环结束后，循环中只改标志
检查传入参数是否没有利用
一些无用#define宏未注释
注意>> 需要空格 > >；比较符号也需要加括号cout << (s1<s2) << 
条件未考虑全（画流程图）
自己的关键if跟题目要求的if不一致
每一个for语句块中出现的变量都要考虑初始化问题
存在跳转分支考虑有些变量落空的情况，assert
取值范围，很可能要long long
double 溢出 10^300 166！
样例输入也是，字符串处理要考虑行尾的空格
跟样例输出要比较，当心空格
跟样例输出要比较，当心空格，当心最后输出位，不用do while 用while(true) + i
考虑语义的顺序执行，没必要把后发生的反过来先执行
下标 0 和 1 的区分（所有的数组，都要考虑一遍是0s还是1s）
for开始结束的边界（所有的for，< ，都要考录是0 还是 1，< 还是<= ）
如果可以，一定要测试极限情况（最少+特例），先自己构造几个用例
尽量用中间变量，不要跳来跳去
map/list尽量用vector替代，迭代器循环尽量用size
！！如果缩写，两个单词首字母相同，不要换其中一个，而是变成两个字母去缩写
【变量命名
调试变量 bufbuf1 bufbuf2
全局变量 ggmaxlay
main变量 gmaxlay gn gm gr gc
局部变量 i j k m n
数据结构 vep1 mae2 lie1 yc4i
迭代器 itv itm itl
【other
reverse != reserve
ij不要跟rc弄混
有些坑，要事先在草稿纸上写好，写到任何想到的，要//注释留空
不用fflush因为直接把文件清空了
浮点数陷阱 不要用== 或者 !=
断点调试空语句，以及想不暴露给全局的都丢花括号里
{
	int i = 0;
}
局部变量
for(int i = 0;i<n;i++){
	int j = i;//【每次都会赋值，不要写成int j;这种
	j = j;
}
//【error 初始化表达式只能定义循环控制变量，不要定义其他
for(int i = 0,float k = 3.5;i<5;i++){
	int j = i;
	j = j;
}
空间限制
#include<iostream>
int main(){
	int a1[500][1000] = {0};//局部变量上限 500*1000
	return 0;
}
#include<iostream>
int a2[64][1000][1000] ={0};//全局变量上限 64*1024*1024
int main(){
	return 0;
}
