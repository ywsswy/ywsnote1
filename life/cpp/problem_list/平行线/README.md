# 题目
![image](https://user-images.githubusercontent.com/25153243/73173468-623b7e80-4140-11ea-861a-717f077d15d5.png)
# 官方题解
![image](https://user-images.githubusercontent.com/25153243/73173525-85662e00-4140-11ea-90c1-22442922d947.png)
![image](https://user-images.githubusercontent.com/25153243/73173556-94e57700-4140-11ea-8641-c44005729c05.png)
![image](https://user-images.githubusercontent.com/25153243/73173574-a464c000-4140-11ea-9987-27ea799b434a.png)
![image](https://user-images.githubusercontent.com/25153243/73173600-b0508200-4140-11ea-873c-3cd4354f52e4.png)

这代码，，，我无力吐槽，全局空间污染严重，使用非标准厍函数__gcd()等等，

这里给出我的代码，其中，有几点需要注意：
## 伪斜率
计算两点连线斜率时，关键在于处理x1 == x2时计算斜率会出现除以零的情况，官方题解中使用了求最大公约数，并构造了make_pair(x/g, y/g)来表示斜率，但是我的方法依然使用double来表示斜率（我通过加了一个偏移量，保证永远不会除以零）
## std::next_permutation
n个点两两配对的排列组合情况，例如ve = {1,4,2,3,5,6}表示第1和第4个点配对成一条线，第2和第3个点配对，第5和第6个点配对。
暴力遍历所有的配对情况，找出最大值，此处使用标准库std::next_permutation获取全排列会超时，因为{1,2,3,4,5,6}的下一个排列是{1,2,3,4,6,5}，显然这两种排列属于同一种情况，{5,6,1,2,3,4}等等很多都重复了，所以参照[std::next_permutation的实现原理](https://en.cppreference.com/w/cpp/algorithm/next_permutation)改写一个出来。
我个人的YNextPermutation实现如下：
```
template<class T> bool YNextPermutation(T begin, T end){
    if (begin == end){
		return false;
	}
	T before_i = end-1;
	while (before_i != begin){
		T i = before_i;
		if (*--before_i < *i){
			std::reverse(i, end);
			while(!(*before_i < *i)){
				++i;
			}
			std::iter_swap(before_i,i);
			return true;
		}
	}
	std::reverse(begin, end);
	return false;
}
```
适用于本题的实现见完整代码：
```
//#define YLFILE
//#define YWATCH
#define YINDEX(x) (x-1)
#include<iostream>
#include<iomanip>
#include<string>
#include<sstream>
#include<fstream>
#include<vector>
#include<list>
#include<map>
#include<algorithm>
//求最大公约数
long long YGcd(long long a, long long b){
	while (a != 0){
		b %= a;
		if (b == 0){
			return a;
		}
		else{
			a %= b;
		}
	}
	return b;
}

//计算两点连线的伪斜率（真实斜率当x1 == x2时计算斜率会出现除以零的情况，而这里加了一个偏移量，保证永远不会除以零），（输入数据要求两点不重合）
double YGetK(long long x1, long long y1, long long x2, long long y2) {
	long long dy = y2 - y1;
	long long dx = x2 - x1 + dy*(200000 + 1);
	long long gcd = YGcd(dx, dy);
	return (double)(dy / gcd) / (dx / gcd);
}

int main() {
	std::cin.tie(0);
	std::ios::sync_with_stdio(false);
#ifdef YLFILE
	std::cin.rdbuf((new std::ifstream("yin1.txt"))->rdbuf());
	//std::cout.rdbuf((new std::ofstream("yout1.txt"))->rdbuf());
#endif
	int n = 0;
	std::cin >> n;
	std::vector<int> gx(n, 0);//0s，每个点的横坐标
	std::vector<int> gy(n, 0);
	std::vector<std::vector<double> > gk(n,std::vector<double>(n,0));//0s，gk[0][1]表示第1个点和第二个点，两点连线的伪斜率
	for (int i = 1; i <= n; ++i) {
		std::cin >> gx[YINDEX(i)] >> gy[YINDEX(i)];
		for (int j = 1; j < i; ++j) {
			gk[YINDEX(i)][YINDEX(j)] = gk[YINDEX(j)][YINDEX(i)] = YGetK(gx[YINDEX(i)], gy[YINDEX(i)], gx[YINDEX(j)], gy[YINDEX(j)]);
		}
	}
	int result = 0;
	std::vector<int> ve(n, 0);//n个点两两配对的排列组合情况，例如ve = {1,4,2,3,5,6}表示第1和第4个点配对成一条线，第2和第3个点配对，第5和第6个点配对
	for (int i = 1; i <= n; i++){
		ve[YINDEX(i)] = i;//[0s] = 1s
	}
	while (true){
		{//这部分是计算当前ve的排列下，有多少对平行线对
			std::map<double, int> count;
			for (int i = 0; i < n; i += 2) {
				++count[gk[YINDEX(ve[i])][YINDEX(ve[i + 1])]];
			}
			int sum = 0;
			size_t ma_size = count.size();
			std::map<double, int>::iterator it = count.begin();
			for (size_t k = 0; k < ma_size; ++k, ++it) {
				sum += (it->second*(it->second - 1) / 2);//4条线斜率相同，则能组成C(4,2)=4*3/2 对平行线对
			}
			result = std::max(result, sum);
		}
		{//这部分是计算ve排列的下一种排列配对方案，参照std::next_permutation的实现原理
			bool continue_flag = false;
			int before_i = n - 1 * 2;//0s
			while (before_i != 0){
				int i = before_i;
				before_i -= 1 * 2;
				if (ve[before_i + 1] < std::max(ve[i], ve[i + 1])){
					std::vector<int> visit(n, 0);
					for (int j = 0; j <= before_i; ++j){
						visit[YINDEX(ve[j])] = 1;//排列中前面的部分不需要改动
					}
					int k = ve[before_i + 1];
					while (visit[YINDEX(++k)] == 1);
					ve[before_i + 1] = k;
					visit[YINDEX(k)] = 1;
					k = 0;
					for (; i < n; ++i){
						while (visit[k++] == 1);
						ve[i] = k;//需要改动的部分改成最小排列即可
					}
					continue_flag = true;
					break;
				}
			}
			if (continue_flag){
				continue;
			}
			else{
				break;
			}
		}
	}
	std::cout << result << '\n';
	return 0;
}
```