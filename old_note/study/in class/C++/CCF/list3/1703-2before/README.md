#include<iostream>
#include<map>
using namespace std;
int main(){
	freopen("in2.txt","r",stdin);
	int gn = 0;
	cin >> gn;
	map<int,int> ma;
	for(int i = 0;i<gn;i++){
		ma[i+1] = i+1;
	}
	int ga = 0;
	cin >> ga;
	for(int i = 0;i<ga;i++){
		int bufin = 0;
		int bufgo = 0;
		cin >> bufin >> bufgo;
		ma[bufin] += bufgo;
		if(bufgo > 0){
			for(map<int,int>::iterator itm=ma.begin();itm!=ma.end();itm++){
				if(
					itm->second >= ma[bufin]-bufgo 
					&& 
					itm->second <= ma[bufin]
					&&
					itm->first != bufin
					){
					itm->second--;
				}
			}
		}
		else{
			for(map<int,int>::iterator itm = ma.begin();itm!=ma.end();itm++){
				if(
					itm->second >= ma[bufin]				
					&&
					itm->second <= ma[bufin]-bufgo
					&&
					itm->first != bufin
					){
					itm->second++;
				}
			}
		}
	}
	for(int i = 0;i<gn;i++){
		for(map<int,int>::iterator itm = ma.begin();itm!=ma.end();itm++){
			if(itm->second == i+1){
				cout << itm->first << ' ';
				break;
			}
		}
	}
	return 0;
}
