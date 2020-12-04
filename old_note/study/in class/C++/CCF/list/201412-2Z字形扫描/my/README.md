#include<iostream>
#include<vector>
using namespace std;
int main(){
	//freopen("in2.txt","r",stdin);
	int n;
	cin >> n;
	vector<vector<int> > v;
	vector<int> vr;
	for(int i = 0;i<n;i++){
		vr.clear();
		for(int j = 0;j<n;j++){
			int buf;
			cin >> buf;
			vr.push_back(buf);
		}
		v.push_back(vr);
	}
	int x = 0;
	int y = 0;
	int dir = 2;
	while(true){
		cout << v[y][x] << ' ';
		if(x == n-1 && y == n-1){
			break;
		}
		if(dir == 2){
			x = x+1;
		}
		else if(dir == 3){
			y = y+1;
		}
		else if(dir == 4){
			x = x-1;
			y = y+1;
		}
		else if(dir == 6){
			x = x+1;
			y = y-1;
		}
		if(dir == 2){
			if(y == 0){
				dir = 4;
			}
			else{
				dir = 6;
			}
		}
		else if(dir == 3){
			if(x == 0){
				dir = 6;
			}
			else{
				dir = 4;
			}
		}
		else if(dir == 4){
			if(y == n-1){
				dir = 2;
			}
			else if(x == 0){
				dir = 3;
			}
		}
		else if(dir == 6){
			if(x == n-1){
				dir = 3;
			}
			else if(y == 0){
				dir = 2;
			}
		}
	}
	return 0;
}
