#include <REGX52.H>	 
#include "board.h"
#include"intrins.h"
#include "stdio.h"	
			
char ystr[20] = "";	 
uchar safe = 1;
uint temperature=0;
uint humitity=0;
uint success_flag=0;
sbit DQ = P2^2;	 
sbit bb = P2^3;
void delay(unsigned int x)    //延时 x ms
{
    unsigned int i,j;
    for(i=x;i>0;i--)
        for(j=110;j>0;j--);
}
void delay_10us(unsigned int t)   //误差 0us
{
	while(t--);
}
/*******************************************************************/
unsigned char DHT_read()
//通过一个带返回值的函数从 DHT11 读取 1 个字节的数据
{
	unsigned char i,dat ,flag,tem;
	for(i=0;i<8;i++) //每次读取一个位
	{
		flag = 2;
		while((!DQ) && flag++);//这里的 flag 是为了防止死循环， 当 flag 溢出至 0 时跳出指令
		delay_10us(3);;//因为数据为 0 的话高电平只维持 26~28us，之后的 50us 内数据线为 0，若
					 //30us 后数据线上为高电平，则证明本数据位为 1
		tem = 0;
		if(DQ)
			tem=1;
		flag=2;
		while((DQ)&&flag++); //等待下一位数据
		if(flag == 1) //当死循环发生时，本次通讯无效
			break;
		dat<<=1;
		dat|=tem;
	}
	return dat; //一个字节的数据通过返回值得到
}
/********************************************************************/
void TH_read()
//温湿度数据读取函数
{
	unsigned char flag,TH_tem,TL_tem,HH_tem,HL_tem,check,sum;
	DQ = 0;
	delay(24);
	DQ = 1; //主机发送开始信号触发一次温湿度采集
	delay_10us(4);
	DQ = 1;
	if(!DQ) //处理 DHT11 的低电平响应信号
	{
		flag = 2;
		while((!DQ) && flag++);
		
		flag = 2;
		while( DQ && flag++);
		HH_tem = DHT_read();
		HL_tem = DHT_read();
		TH_tem = DHT_read();
		TL_tem = DHT_read();
		check = DHT_read();//依次读出 8bit 湿度整数数据+8bit 湿度小数数据+8bi 温度整数数据+8bit 温
						   //度小数数据+8bit 校验和
		DQ = 1; //拉高数据线， DHT11 进入待机状态
				//以下为求和校验部分
		sum=(HH_tem+HL_tem+TH_tem+TL_tem);
		if(sum == check)
		{
			success_flag++;						   
			temperature = TH_tem; //全局变量的温度采集
			humitity = HH_tem; //全局变量的湿度采集
								//因小数位为 0，省略
		}
	}
}
void yTestDis(){
	char ystr[20] = "";
	yFtoa(gt0,ystr);
	yStr_1602(YFIRSTLINE1602,ystr);
}
void yRead(){ 
	char i = 0;	 
	delay(1000);
	TH_read();
	if(success_flag > 1){
		yUSRT1_init();	
		for(i = 0;i<10;i++){
			ySend_byte(temperature);	
			delay(5);
			safe = yRead_byte();
			delay(5);
		}
		yUSRT1_disa();
		
		yItoa(temperature,ystr,10);
		yStr_1602(YSECONDLINE1602,ystr);
		yItoa(safe,ystr,10);
		yStr_1602(YFIRSTLINE1602,ystr);
	}
}	
void main(){	
	bb = 1;	  
	YLED = YLED_OFF;
	yBoard_init();
	y1602_init(); 
	yT0_init();	
	YLED = YLED_ON;
	delay(1000);
	YLED = YLED_OFF;
	while(1){
		yRead();
		if(safe == 'd'){
			YLED = YLED_ON;
			bb = 0;
			//报警
		}
		else{
			//安全
			YLED = YLED_OFF;
			bb = 1;
		}
	}
}
