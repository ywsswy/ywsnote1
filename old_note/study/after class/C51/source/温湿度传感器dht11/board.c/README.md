#include "Yboard.h"
#include <string.h>
#define YT1 1
sbit glcden1602 = P2^7; 				//命令使能口
sbit glcdrw1602 = P2^6;					//读写选择口   //unuse
sbit glcdrs1602 = P2^5;					//命令调节口 
sbit speaker = P2^0;	//。。//
	
void yWrite_com1602(uchar com);
void yWrite_dat1602(uchar dat);
float gt0 = 0.0;
/*pro1		引脚低功耗初始化*/
void yBoard_init(){
	P0=0;
	P1=0;
	YLED = YLED_OFF;
	YUPKEY = YKEY_LOOSE;
	YDOWNKEY = YKEY_LOOSE;
}				
/*pro2		1602液晶显示*/
void yWrite_dat1602(uchar dat){	
	uchar c,j;
	for(c = 0,j = 0;j<8;j++){		//倒序转换
		c |= (dat&1<<j)>>j<<7-j;
	}
	glcdrs1602 = 0;
	yDelay(YT1);
	glcdrs1602 = 1;
	P0 = c;		
	yDelay(YT1);
	glcden1602 = 1;
	yDelay(YT1);
	glcden1602 = 0;		
}	  	
/*pro3		1602液晶命令*/
void yWrite_com1602(uchar com){
	uchar c,j;
	for(c = 0,j = 0;j<8;j++)		 //倒序转换
		c |= (com&1<<j)>>j<<7-j;
	glcdrs1602 = 0;
	P0 = c;
	yDelay(YT1);
	glcden1602 = 1;	  
	yDelay(YT1);
	glcden1602 = 0;
}	 
/*pro4		1602初始化*/
//1602 2*16个字符
void y1602_init(){
	glcden1602 = 0;
	yWrite_com1602(0x38); 
    //显示模式设置  00111000  设置16*2显示  5*7点阵  8位数据接口
	yWrite_com1602(0x0f);	 
    //显示开关及光标设置   00001DCB 
    //D=1，开显示     D=0， 关显示 
    //C=1，显示光标   C=0，不显示光标 
    //B=1，光标闪烁   B=0，光标不闪烁
	yWrite_com1602(0x06);
	//地址指针自动+1且光标+1，写字符屏幕不会移动
	yWrite_com1602(0x01);
}
/*pro5		1602显示字符串*/	
void yStr_1602(uchar loc,char*s){
	uchar i;
	uchar len = strlen(s);
	yWrite_com1602(loc);
	for(i = 0;i<len;i++){
		yWrite_dat1602(s[i]);
	} 
}	
/*pro6		float浮点类型数保存为字符串*/
void yFtoa(float num,char *str){
	char s[24] = "#######################";
	char buf[8] = "";
	char flag = 0;
	float temp = 1.0;
	int i = 0;
	int a = 0;
	int len = 0;
	int lenb = 0;
	char locd = 0;
	char loch = 0;
	if(num < 0){
		flag = 1;
		num = -num;
		s[0] = '-';
	}
	if((int)(num/100000000000000.0)!=0){
		while(1);
	}
	for(temp = 1000000000000.0,i = 3;flag+len<17;i--,temp = temp/10000.0){
		a = (int)(num/temp);
		if(a != 0){
			if(len == 0){
				yItoa(a,s+flag,10);
				len = strlen(s+flag);
			}
			else{
				yItoa(a,buf,10);
				lenb = strlen(buf);
				if(lenb != 4){
					s[len+flag] = '0';len++;
					if(lenb != 3){
						s[len+flag] = '0';len++;
						if(lenb != 2){
							s[len+flag] = '0';len++;
						}
					}
				}
				strcpy(s+len+flag,buf);
				len+=strlen(buf);
			}
			num = num - temp*(float)(a);
		}
		else if(len != 0){
			s[len+flag] = '0';len++;
			s[len+flag] = '0';len++;
			s[len+flag] = '0';len++;
			s[len+flag] = '0';len++;
		}
		if(i == 0){
			if(len == 0){
				s[len+flag] = '0';len++;
			}
			s[len+flag] = '.';len++;
		}
	}
	lenb = strlen(s);
	locd = 127;
	loch = 127;
	for(i=flag;i<lenb;i++){
		if(s[i] == '.' && locd == 127){
			locd = i;
		}
		else if(s[i] != '.' && s[i] != '0' && loch == 127){
			loch = i;
		}
	}
	if(locd-loch > 5){
		s[flag+locd-loch+2] = 0;
	}
	else if(locd-loch > 0){
		s[8] = 0;
	}
	else if(locd-loch >= -8){
		s[8+flag+loch-locd] = 0;
	}
	else
		s[16+flag] = 0;
	strcpy(str,s);
} 
/*pro7		int整数类型数保存为字符串*/
void yItoa(int num,char*str,char ra){
	uchar index[]="0123456789ABCDEF",temp;
	unsigned unum;
	int i=0,j,k;
	if(num<0){
		unum=(unsigned)-num;
		str[i++]='-';
		k=1;
	}
	else {
		unum=(unsigned)num;
		k=0;
	}
	do{
		str[i++]=index[unum%ra];
		unum/=ra;
	}while(unum);
	str[i]='\0';
	for(j=k;j<=(i-1)/2;j++){
		temp=str[j];
		str[j]=str[i-1+k-j];
		str[i-1+k-j]=temp;
	}
}  	
/*pro8		单纯粗略延时（12M => 1us）*/ 		 
void yDelay(uchar t){
	while(t-->0);
}
/*pro9		T0定时,往gt0上累加(11.0952MHz)*/
void gt0_int(void) interrupt TF0_VECTOR{
//	gt0 += 0.071227174;
}
/*pro10		T0定时器初始化，全局之首!(so you can see EA)，方式1，16位12M=>65.536ms  */
void yT0_init(){ 
	TMOD |= T0_M0_;
	TMOD &= ~(T0_M1_+T0_CT_+T0_GATE_);
	EA = 1;
	//ET0 = 1;
	//TR0 = 1;
}
/*pro11		发送数据(在pro12-串口方式1下)*/
void ySend_byte(uchar dat){
	SBUF = dat;
}
/*pro12		串口方式1初始化，11.0592MHz+9600波特率+T1定时器*/ 
void yUSRT1_init(){
	//SCON
	SM0 = 0;
	SM1 = 1;
	SM2 = 0;
	REN = 1;
	//TMOD 方式2
	TMOD |= T1_M1_;
	TMOD &= ~(T1_M0_+T1_CT_+T1_GATE_);
	TH1 = 0xfd;
	TL1 =0xfd;		   
	ET1 = 1;	
	TR1 = 1;
}	 
/*pro13		
MODgt11		T1定时,蓝牙，do nothing
MODgt12		P1_1翻转 闹钟*/
void gt1_int(void) interrupt TF1_VECTOR{
#ifdef MODgt11
#endif
#ifdef MODgt12
	//speaker = ~speaker;
#endif
}
/*pro14		禁止串口方式1*/
void yUSRT1_disa(){	   
	ET1 = 0;	
	TR1 = 0;
	RI = 0;
	TI = 0;	
}
/*pro15		T1定时（MODgt12）初始化*/
void yT1_init(){  
	TMOD |= (0x20);
	TMOD &= ~(0x10);
	TH1 = 0x80;
	TL1 =0x80;	   
	ET1 = 1;	
	TR1 = 1;	
	speaker = 1;
	P1 = 0x55;
}
/*pro16		T1定时禁止（MSDgt12）*/
void yT1_disa(){  
	ET1 = 0;	
	TR1 = 0;
}
/*pro17		接收数据(在pro12-串口方式1下)*/
uchar yRead_byte(){
	uchar buf = SBUF;
	RI = 0;
	return buf;
}
