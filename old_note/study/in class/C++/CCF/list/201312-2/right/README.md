#include<iostream>
#include<cstdio>
#include<string>
#include<vector>
#include<deque>
#include<cstring>
#include<list>
#include<map>
using namespace std;
int main(){
	freopen("in2.txt","r",stdin);
	string s;
	int a[16];
	cin>>s;
	for(int i = 0;i<13;i++){
		a[i] = s[i]-'0';
	}
	int sum = 0;
	for(int i = 0,flagj = 0;i<12;i++){
		if(a[i] >= 0){
			flagj++;
			sum+=flagj*a[i];
		}
	}
	char code = (sum%11==10) ? 'X' : sum%11+'0';
	if(code == s[12]){
		cout<<"Right";
	}
	else{
		s[12]=code;
		cout << s;
	}
	return 0;
}

