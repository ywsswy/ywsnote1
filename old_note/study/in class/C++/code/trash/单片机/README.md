#include<reg52.h>  
#define uchar unsigned char
#define uint unsigned int
char disdu[16] = {0x3f,0x06,0x5b,0x4f,0x66,0x6d,0x7d,0x07,0x7f,0x6f,0x77,0x7c,0x39,0x5e,0x79,0x71};
sbit key_add = P3^6;//s4 P1^6
sbit dula = P2^6;
sbit wela = P2^7;
uint rate = 5;
void main(){
	while(1){
		if(key_add == 0)
			rate++;
		P0 = 0xff;//必要，送位选数据前关闭所有显示，防止打开位选锁存时原来段选数据通过位选锁存器造成混乱。
		wela = 1;	   
		P0 = 0x00;
		wela = 0;	
		dula = 1;
		P0 = disdu[rate%10];
		dula = 0;
	}
}
