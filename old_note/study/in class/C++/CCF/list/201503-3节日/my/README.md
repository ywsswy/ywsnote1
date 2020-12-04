#include<iostream>
using namespace std;
int gdp[12] = {31,28,31,30,31,30,31,31,30,31,30,31};
int gdr[12] = {31,29,31,30,31,30,31,31,30,31,30,31};
int yCaile(int year,int month,int day){
	int century;
	int buf;
	if (month == 1 || month == 2){
		month = month + 12;
		year--;
	}
	century = year/100;
	year = year%100;
	buf = year + year/4 + century/4 - 2*century + 26*(month + 1)/10 + day - 1;
	while(buf < 0){
		buf += 7;
	}
	return buf%7;
}
bool yLeap(int year){
	if(year%400 == 0){
		return true;
	}
	else if(year%100 == 0){
		return false;
	}
	else if(year%4 == 0){
		return true;
	}
	else{
		return false;
	}
}
int main(){
	freopen("in2.txt","r",stdin);
	int g1,g2,g3;
	int gy1,gy2;
	cin >> g1 >> g2 >> g3 >> gy1 >> gy2;
	int gybuf = min(gy1,gy2);
	gy2 = max(gy1,gy2);
	gy1 = gybuf;
	for(int y = gy1;y<=gy2;y++){
		int num = 0;
		int j = 0;
		if(yLeap(y)){
			for(j = 0;j<gdr[g1-1];j++){
				if(yCaile(y,g1,j+1) == g3%7){
					num++;
					if(num == g2){
						break;
					}
				}
			}
		}
		else{
			for(j = 0;j<gdp[g1-1];j++){
				if(yCaile(y,g1,j+1) == g3%7){
					num++;
					if(num == g2){
						break;
					}
				}
			}
		}
		if(num == g2){
			cout << y << '/';
			cout.fill('0');
			cout.width(2); 
			cout << g1 << '/';
			cout.fill('0');
			cout.width(2); 
			cout<< j+1 << endl;
		}
		else{
			cout << "none" << endl;
		}
	}
	return 0;
}
