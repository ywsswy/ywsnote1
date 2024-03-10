frz带着M元钱来到最近的礼物商店
商店有N种礼物,购买一个第i种礼物需要Wi元钱
购买第i种礼物x个,她还会得到Ai * x + Bi(x > 0)颗糖果。
求可获得最多的糖果
【单组数据
输入包括N+1行,第一行包括两个由空格隔开的正整数M和N(1 ≤ M ≤ 1000, 1 ≤ N ≤ 1000),表示frz所拥有的钱和礼物的种数。
接下来N行,每行三个由空格隔开的正整数Wi, Ai, Bi(1 ≤ Wi ≤ 1000, 0 ≤ Ai, Bi ≤ 1000),表示第i种礼物的价格和赠送糖果的参数。
【input
100 2
10 2 1
20 1 1
【output
21
【//hint拆分成Ai+Bi的01背包 和 Ai*(x-1)的完全背包
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
	freopen("d2.txt","r",stdin);
	int gn = 0;
	int gm = 0;
	cin >> gm >> gn;
	int mc[2001] = {0};
	int mw[2001] = {0};
	for(int i = 0;i<gn;i++){
		int buf1 = 0;//cost
		int buf2 = 0;//value1
		int buf3 = 0;//value2
		cin >> buf1 >> buf2 >> buf3;
		mc[i+1] = buf1;
		mw[i+1] = buf2;
		mc[i+1+gn] = buf1;
		mw[i+1+gn] = buf2+buf3;
	}
	int f[2004] = {0};
	for(int i = 0;i<=gn*2;i++){
		f[i] = 0;
	}
	for(int i = 0;i<gn*2;i++){
		if(i+1>gn){//01背包
			for(int j = 0;j<gm-mc[i+1]+1;j++){
				f[gm-j] = max(f[gm-j],f[gm-j-mc[i+1]]+mw[i+1]);
			}
		}
		else{//完全背包
			for(int j = 0;j<gm-mc[i+1]+1;j++){
				f[j+mc[i+1]] = max(f[j+mc[i+1]],f[j]+mw[i+1]);
			}
		}
	}
	cout << f[gm];
	return 0;
}
