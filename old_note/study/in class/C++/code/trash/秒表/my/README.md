#include <stdio.h>
#include <conio.h>
#include <windows.h>
int minutes = 0;
int seconds = 0;
int hscd;
void update(){
	Sleep(10);
	hscd++;
	if (hscd==100){
		hscd=0;  
		seconds++;  
	}
	if (seconds==60){  
		seconds=0;  
		minutes++;  
	}
}
void display(){
	printf("\r");
	printf("%d:%d:%d",minutes,seconds,hscd);
}
int main(){
	int a;
	a = 1;
	while(a){
		if(kbhit()){
			getch();
			a = 0;
		}
	}
	a = 1;
	while(a){
		if(kbhit()){
			getch();
			break;
		}
		update();
		display();
	}
	return 0;
}
