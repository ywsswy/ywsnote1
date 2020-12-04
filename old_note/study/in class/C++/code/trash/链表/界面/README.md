//全部用地址操作
#include <stdio.h>
#include <windows.h>
#include <conio.h>
#include <time.h>
#define ROW 15
#define COL 20
void gotoxy(int x,int y){
	COORD coord;
	coord.X=x;
	coord.Y=y;
	SetConsoleCursorPosition( GetStdHandle( STD_OUTPUT_HANDLE ), coord );
}
void Draw(){ 
	int i = 0;
	int j = 0;
	//上边界
	for(i = 1;i<=COL;i++){
		gotoxy(i,1);
		printf("*");
	}
	//下边界
	for(i = 1;i<=COL;i++){
		gotoxy(i,ROW);
		printf("*");
	}
	//左边界
	for(i = 1;i<=ROW;i++){
		gotoxy(1,i);
		printf("*");
	}
	//右边界
	for(i = 1;i<=ROW;i++){
		gotoxy(COL,i);
		printf("*");
	}
}
int main(){
	int myy = ROW-1;
	int myx = COL/2;
	srand(time(NULL));
	Draw();
	gotoxy(myx,myy);
	printf("&");
	while(1);
	return 0;
}
