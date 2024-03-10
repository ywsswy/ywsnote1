#include<iostream>
using namespace std;
int main(){
	int gn = 0;
	int gk = 0;
	cin >> gn >> gk;
	int sum = 0;
	int gp = 0;
	for(int i = 0;i<gn;i++){
		int buf = 0;
		cin >> buf;
		sum += buf;
		if(sum >= gk){
			gp++;
			sum = 0;
		}
	}
	if(sum != 0){
		gp++;
	}
	cout << gp;
	return 0;
}
