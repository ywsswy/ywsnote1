//输入数据首先包含一个正整数C，表示有C组测试用例，每组测试用例的第一行是两个整数n和m(1<=n<=100, 1<=m<=100),分别表示经费的金额和大米的种类，然后是m行数据，每行包含3个数p，h和c(1<=p<=20,1<=h<=200,1<=c<=20)，分别表示每袋的价格、每袋的重量以及对应种类大米的袋数。
对于每组测试数据，请输出能够购买大米的最多重量，你可以假设经费买不光所有的大米，并且经费你可以不用完。每个实例的输出占一行。
【input
1
8 2
2 100 4
4 100 2
【output
400
//////////////////////////////////////////////////////////////////////
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
	//freopen("in2.txt","r",stdin);
	int gnn = 0;
	cin >> gnn;
	for(int gi = 0;gi<gnn;gi++){
		int f[104] = {0};
		int c[104] = {0};
		int w[104] = {0};
		int t[104] = {0};
		int gn = 0;
		int gm = 0;
		cin >> gn >> gm;
		for(int i = 0;i<gm;i++){
			cin >> c[i+1] >> w[i+1] >> t[i+1];
		}
		for(int i = 0;i<=gn;i++){
			f[i] = 0;
		}
		for(int i = 0;i<gm;i++){
			for(int j = 0;j<gn-c[i+1]+1;j++){
				for(int k = 0;k<t[i+1];k++){
					if(gn-j-(k+1)*c[i+1] >= 0){
						f[gn-j] = max(f[gn-j],f[gn-j-(k+1)*c[i+1]]+(k+1)*w[i+1]);
					}
				}
			}
		}
		cout << f[gn] << endl;
	}
	return 0;
}
