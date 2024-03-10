#include<iostream>
#include<map>
#include<set>
using namespace std;
int main(){
	//freopen("in2.txt","r",stdin);
	map<int,set<int> > m;
	int n;
	cin >> n;
	for(int i = 0;i<n;i++){
		int x1;
		int x2;
		int y1;
		int y2;
		cin >> x1 >> y1 >> x2 >> y2;
		for(int j = y1;j<y2;j++){
			for(int k = x1;k<x2;k++){
				m[j].insert(k);
			}
		}
	}
	int num = 0;
	for(map<int,set<int> >::iterator it = m.begin();it!=m.end();it++){
		num+=it->second.size();
	}
	cout << num;
	return 0;
}
