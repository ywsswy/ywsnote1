【wrong.exe
【right.exe
【data.exe
#include<iostream>
#include<cstdlib>
#include<ctime>
using namespace std;
#define PAR1 30//路由器数量上限
#define PAR2 20//电脑数量上限
int main(){
	srand(static_cast<unsigned>(time(NULL)));
	int par1 = 0;//路由器数量
	int par2 = 0;//电脑数量
	par1 = rand()%PAR1+1;
	par2 = rand()%(PAR2+1);	
	cout << par1 << ' ' << par2 << endl;
	for(int j = 0;j<par1-1;j++){
		cout << rand()%(j+1)+1 << ' ';
	}
	cout << endl;
	for(int j = 0;j<par2;j++){
		cout << rand()%par1+1 << ' ';
	}
	return 0;
}
【cmd
F:\projects\cpp\ccf1\Debug>data.exe
25 10
1 2 2 4 3 2 1 3 8 1 11 11 13 13 3 16 16 9 6 14 15 1 19 5 
2 6 19 21 12 21 17 4 13 24 
F:\projects\cpp\ccf1\Debug>data.exe > in.txt
F:\projects\cpp\ccf1\Debug>right.exe < in.txt
11
F:\projects\cpp\ccf1\Debug>wrong.exe < in.txt
8
F:\projects\cpp\ccf1\Debug>
