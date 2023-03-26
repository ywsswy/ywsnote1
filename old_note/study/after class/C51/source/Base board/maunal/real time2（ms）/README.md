#include <REGX52.H>	 
#include "Yboard.h"
void yTestDis(){
	char ystr[20] = "";
	yFtoa(gt0,ystr);
	yStr_1602(YFIRSTLINE1602,ystr);
}	 
void main(){
	yBoard_init();
	y1602_init(); 
	yT0_init();
	while(1){
		if(YUPKEY==YKEY_PRESS){
			yTestDis();
		}
	}
}

