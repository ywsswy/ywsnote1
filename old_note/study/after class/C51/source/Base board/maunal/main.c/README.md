#include <REGX52.H>	 
#include "Yboard.h"
char gt;
void yTestDis(){
	char ystr[20] = "";
	//yItoa(gt,ystr);
	yFtoa(gt0,ystr);
	yStr_1602(YFIRSTLINE1602,ystr);
}	 
void main(){
	yBoard_init();
	y1602_init(); 
	yT0_init();	 
	yUSRT1_init();	 
	P1 = 0xf0;
	while(1){
	
		if(YUPKEY==YKEY_PRESS){
			yTestDis();
		}
		if(YDOWNKEY==YKEY_PRESS){
			ySend_byte(0xf0);
		}
		else if(YUPKEY==YKEY_PRESS){
			yUSRT1_disa();
		}
		else{
			ySend_byte(0x00);
		}		
	}
}

