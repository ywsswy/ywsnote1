#include<REGX52.H>	   
#define uint unsigned int
#define uchar unsigned char
#define YFIRSTLINE1602 0x80
#define YSECONDLINE1602 0xc0
#define YLED_ON 0
#define YLED_OFF 1	
#define YKEY_LOOSE 1
#define YKEY_PRESS 0
#define MODgt11	
						
sbit YUPKEY = P3^6;
sbit YDOWNKEY = P3^7;
sbit YLED = P1^0;
		  
extern float gt0;
//-32768~32767
extern void yItoa(int num,char*str,char ra);
extern void yFtoa(float num,char *str);				
//uchar loc should between([YFIRSTLINE1602~YFIRSTLINE1602+15]U[YSECONDLINE1602~YSECONDLINE1602+15])
extern void yStr_1602(uchar loc,char*str);
extern void yBoard_init();
extern void yDelay(uchar t);
extern void y1602_init();
extern void yT0_init();
extern void yUSRT1_init();
extern void ySend_byte(uchar dat);
extern uchar yRead_byte();
extern void yUSRT1_disa();
extern void yT1_init();
extern void yT1_disa();
