#include<iostream>
#include<algorithm>
using namespace std;
int main(){
	freopen("in2.txt","r",stdin);
	int t = 0;
	cin >> t;
	int v[7] = {
		3500,
		1500,
		4500,
		9000,
		35000,
		55000,
		80000,
	};
	double lv[8] = {
		0,
		0.03,
		0.1,
		0.2,
		0.25,
		0.3,
		0.35,
		0.45,
	};
	int k[7] = {
		0,
		1500*0.03,
		(4500-1500)*0.1,
		(9000-4500)*0.2,
		(35000-9000)*0.25,
		(55000-35000)*0.3,
		(80000-55000)*0.35,
	};
	for(int i = 0;i<6;i++){
		v[i+1] = v[i+1]+v[0];
		k[i+1] = k[i+1]+k[i];
	}
	int r[7] = {0};//实际获得门槛
	for(int i = 0;i<7;i++){
		r[i] = v[i]-k[i];
	}
	int ori = 0;
	int flag = 0;//0 未计算
	if(t<r[0]){
		ori = t;
		flag = 1;
	}
	else{
		for(int i = 0;i<6;i++){
			if(t<r[i+1]){
				ori = v[i]+(t-r[i])/(1-lv[i+1]);
				flag = 1;
				break;
			}
		}
	}
	if(flag == 0){
		ori = v[6] + (t-r[6])/(1-lv[7]);
	}
	cout << ori << endl;
	return 0;
}
