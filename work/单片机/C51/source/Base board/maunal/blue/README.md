#include <REGX52.H>	 
#include "Yboard.h"
				  
char ystr[20] = "";
uchar yc = 0x41;
void yTestDis(){
	yFtoa(gt0,ystr);
	yStr_1602(YFIRSTLINE1602,ystr);
}	 
void main(){
	yBoard_init();
	y1602_init(); 
	yT0_init();	 
	yUSRT1_init();	 
	YLED = YLED_ON;
	while(1){
		if(YUPKEY==YKEY_PRESS){
			yTestDis();
		}
		if(YDOWNKEY==YKEY_PRESS){
			ySend_byte(0xf0);
		}
		else{	
			yc = yRead_byte()+16;
			yItoa(yc,ystr);
			yStr_1602(YSECONDLINE1602,ystr);
		}
	}
}

