#include<iostream>
#include<map>
using namespace std;
int main(){
	//freopen("in2.txt","r",stdin);
	int n;
	cin >> n;
	map<int,int> m;
	for(int i = 0;i<n;i++){
		int buf;
		cin >> buf;
		if(m.count(buf)){//if(m.find(buf) != m.end()){
			m[buf]++;
		}
		else{
			m[buf] = 1;
		}
		cout << m[buf] << ' ';
	}
	return 0;
}
