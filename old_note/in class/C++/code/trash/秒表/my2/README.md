#include<stdio.h>
#include<stdlib.h>
#include<conio.h>
#include<windows.h>
int main(){
	int min = 0;
	int sec = 0;
	int buzhidao = 0;
	int a = 1;
	//等回车
	while(a){
		if(kbhit()){
			getch();
			a = 0;
		}
	}
	//开始计时
	a = 1;
	while(a){
		Sleep(10);
		buzhidao = buzhidao+1;
		if(buzhidao == 100){
			sec = sec+1;
			buzhidao = 0;
		}
		if(sec == 60){
			min = min+1;
			sec = 0;
		}
		printf("\r%d:%d:%d",min,sec,buzhidao);
		//等停止
		if(kbhit()){
			getch();
			a = 0;
		}
	}
	return 0;
}
