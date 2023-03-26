3 10
3 4
4 5
5 6
//【1s
3种物品，10容量空间
重量3 价值4
#include<iostream>
#include<map>
#include<algorithm>
using namespace std;
int main(){
	freopen("in2.txt","r",stdin);
	int gn = 0;
	int gm = 0;
	int gc = 0;
	cin >> gn >> gm;
	map<int,int> mc;
	map<int,int> mw;
	for(int i = 0;i<gn;i++){
		int buf1 = 0;
		int buf2 = 0;
		cin >> buf1 >> buf2;
		mc[i+1] = buf1;
		mw[i+1] = buf2;
	}
	map<int,int> f;
	
	for(int i = 0;i<=gm;i++){
		f[i] = 0;
	}
	for(int i = 0;i<gn;i++){
		for(int j = 0;j<gm-mc[i+1]+1;j++){
			f[gm-j] = max(f[gm-j],f[gm-j-mc[i+1]] + mw[i+1]);
		}
	}
	cout << "01背包（可不装满）最大容量为" << gm << "，最大价值为" << f[gm] << endl;
	f[0] = 0;
	for(int i = 0;i<gm;i++){
		f[i+1] = -2147483648;
	}
	for(int i = 0;i<gn;i++){
		for(int j = 0;j<gm-mc[i+1]+1;j++){
			f[gm-j] = max(f[gm-j],f[gm-j-mc[i+1]]+mw[i+1]);
		}
	}
	int ans = f[0];
	for(int i = 0;i<gm;i++){
		ans = max(ans,f[i+1]);
	}
	cout << "01背包（恰好装满）最大容量为" << gm << "，最大价值为" << ans << endl;
	for(int i = 0;i<=gm;i++){
		f[i] = 0;
	}
	for(int i = 0;i<gn;i++){
		for(int j = 0;j<gm-mc[i+1]+1;j++){
			f[j+mc[i+1]] = max(f[j+mc[i+1]],f[j] + mw[i+1]);
		}
	}
	cout << "完全背包（可不装满）最大容量为" << gm << "，最大价值为" << f[gm] << endl;
	f[0] = 0;
	for(int i = 0;i<gm;i++){
		f[i+1] = -2147483648;
	}
	for(int i = 0;i<gn;i++){
		for(int j = 0;j<gm-mc[i+1]+1;j++){
			f[j+mc[i+1]] = max(f[j+mc[i+1]],f[j]+mw[i+1]);
		}
	}
	ans = f[0];
	for(int i = 0;i<gm;i++){
		ans = max(ans,f[i+1]);
	}
	cout << "完全背包（可不装满）最大容量为" << gm << "，最大价值为" << ans << endl;
	return 0;
}
