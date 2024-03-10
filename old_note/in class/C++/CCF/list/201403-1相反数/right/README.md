#include<iostream>
#include<cstdio>
#include<string>
#include<vector>
#include<list>
#include<map>
#include<deque>
#include<stack>
#include<cmath>//要包含
#include<cstring>
using namespace std;
int main(){
	//freopen("in2.txt","r",stdin);
	int n;
	cin >> n;
	map<int,int> m;
	for(int i = 0;i<n;i++){
		int buf;
		cin >> buf;
		m[abs(buf)]++;
	}
	int total = 0;
	for(map<int,int>::iterator it = m.begin();it!=m.end();it++){
		if(it->second > 1){
			total++;
		}
	}
	cout << total;
	return 0;
}
