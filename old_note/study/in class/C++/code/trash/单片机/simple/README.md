#include<reg52.h>  
#define uchar unsigned char
#define uint unsigned int
sbit dula = P2^6;
sbit wela = P2^7;
uint rate = 5;
int main(){
	while(1){
		P0 = 0xff;//必要，送位选数据前关闭所有显示，防止打开位选锁存时原来段选数据通过位选锁存器造成混乱。
		wela = 1;	   
		P0 = 0x00;
		wela = 0;	
		dula = 1;
		P0 = 0x5b;
		dula = 0;
	}
	return 0;
}
