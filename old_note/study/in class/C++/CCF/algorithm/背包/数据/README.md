#include<iostream>
#include<ctime>
using namespace std;
int main(){
	srand(time(NULL));
	int gm = rand()%1000+1;
	int gn = rand()%1000+1;
	cout << gm << ' ' << gn << endl;
	for(int i = 0;i<gn;i++){
		int m = rand()%1000+1;
		int a = rand()%1001;
		int b = rand()%1001;
		cout << m << ' ' << a << ' ' << b << endl;
	}
	return 0;
}
