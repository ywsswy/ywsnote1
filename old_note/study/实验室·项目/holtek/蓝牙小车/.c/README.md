#include <REGX52.H>	 
#include "Yboard.h"
			 
#define MM 11
#define NN 5
sbit IN1=P1^1;
sbit IN2=P1^2;
sbit IN3=P1^3;
sbit IN4=P1^4;
	
char ystr[20] = "";
char yrc = 0;
void yTestDis(){
	yFtoa(gt0,ystr);
	yStr_1602(YFIRSTLINE1602,ystr);
}	 
void dd(){
	unsigned char ii = 0;
	for(ii=0;ii<NN;ii++){
		IN1=0;		
		IN2=0;
		IN3=0; 
		IN4=0;
	}
} 
void head(){
	unsigned char i;
	for(i=0;i<MM;i++){
		IN1=1;
		IN2=0;
		IN3=1;	 
		IN4=0;
	}
	dd();
}
void back(){
	unsigned char i;
	for(i=0;i<MM;i++){
		IN1=0;
		IN2=1;
		IN3=0;	 
		IN4=1;
	}
	dd();
}
void left(){
    unsigned char i;
	for(i=0;i<MM;i++){
		IN1=0;	
		IN2=0;
		IN3=1;
		IN4=0;
	}
	dd();
}
void right(){
	unsigned char i;
	for(i=0;i<MM;i++){
		IN1=1;	 
		IN2=0;
		IN3=0;
		IN4=0;
	}
	dd();
}
void main(){
	yBoard_init();
	y1602_init(); 
	yT0_init();	 
	yUSRT1_init();	 
	YLED = YLED_ON;
	while(1){
		yrc = yRead_byte();
		ystr[0] = yrc;
		ystr[1] = 0;
		//yStr_1602(YFIRSTLINE1602, ystr);
		if(yrc == 0x01){
			head();
		}
		else if(yrc == 0x02){
			back();
		}
		else if(yrc == 0x03){
			right();
		}
		else if(yrc == 0x04){
			left();
		}
		ySend_byte(yrc);
	}
}

