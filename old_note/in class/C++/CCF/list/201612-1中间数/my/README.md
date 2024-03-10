#include<iostream>
#include<map>
using namespace std;
int main(){
	//freopen("in2.txt","r",stdin);
	int n = 0;
	cin >> n;
	map<int,int> ma;//每个数的数量
	for(int i = 0;i<n;i++){
		int buf = 0;
		cin >> buf;
		ma[buf]++;
	}
	map<int,int> ma2;//比每个数小的数量
	int sum = 0;
	int flag = 0;//0:没找到
	for(map<int,int>::iterator itm = ma.begin();itm!=ma.end();itm++){
		if(sum*2 + itm->second == n){
			cout << itm->first;
			flag = 1;
			return 0;
		}
		sum += itm->second;
	}
	if(flag == 0){
		cout << -1;
	}
	return 0;
}
