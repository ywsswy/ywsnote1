#include<iostream>
#include<vector>
using namespace std;
int main(){
	//freopen("in2.txt","r",stdin);
	int gr;
	int gc;
	cin >> gr >> gc;
	vector<vector<int> > v;
	vector<int> vr;
	for(int i = 0;i<gr;i++){
		vr.clear();
		int buf;
		for(int j = 0;j<gc;j++){
			cin >> buf;
			vr.push_back(buf);
		}
		v.push_back(vr);
	}
	for(int j = gc-1;j>=0;j--){
		for(int i = 0;i<gr;i++){
			cout << v[i][j] << ' ';
		}
		cout << endl;
	}
	return 0;
}
