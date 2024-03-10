#include<iostream>
#include<cstdio>
#include<string>
#include<vector>
#include<map>
#include<deque>
#include<list>
#include<cstring>
using namespace std;
long long s[1004][6] = {0};
int mod = 1000000007;
int main(){
	//2>3
	//0>1
	//0:2	013
	//1:20	13
	//2:23	01
	//3:201	3
	//4:230	1
	//5:*	
	int n;
	cin >> n;
	s[0][0] = 1;
	for(int i = 1;i<n;i++){
		int j = i-1;
		s[i][0] = (s[j][0]*1)%mod;
		s[i][1] = (s[j][0]*1+s[j][1]*2)%mod;
		s[i][2] = (s[j][0]*1+s[j][2]*1)%mod;
		s[i][3] = (s[j][1]*1+s[j][3]*2)%mod;
		s[i][4] = (s[j][1]*1+s[j][2]*1+s[j][4]*2)%mod;
		s[i][5] = (s[j][3]*1+s[j][4]*1+s[j][5]*2)%mod;
	}
	cout<<s[n-1][5];
	return 0;
}
