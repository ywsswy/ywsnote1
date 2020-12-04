#include<iostream>
#include<set>
using namespace std;
int main(){
	freopen("in2.txt","r",stdin);
	set<int> s;
	int n;
	cin >> n;
	for(int i = 0;i<n;i++){
		int buf;
		cin >> buf;
		s.insert(buf);
	}
	int bit = -2147483648;
	int num = 0;
	for(set<int>::iterator it = s.begin();it != s.end();it++){
		if(*it == bit+1){
			num++;
		}
		bit = *it;
	}
	cout << num;
	return 0;
}
