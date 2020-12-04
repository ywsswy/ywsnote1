#include <msp430f5529.h>
#include <stdio.h>
#include "HAL_PMM.h"
#include "HAL_UCS.h"
#include "HAL_Board.h"
#include "HAL_Buttons.h"
#include "HAL_Cma3000.h"
#include "HAL_Dogs102x6.h"
#include "stdlib.h"
long int sum_200;     //sum采样200次的和
int num1=0,i,j,k,m;
float avrg;			//最后采样的平均值
char rs[8]="0";		//用显示的字符
int key2=0,key2_2=0/*,key1=0*/;
char s1[] = " DC_V: ";
char s2[] = " DC_A: ";
char s3[] = " AC_Vrms: ";
char s4[] = " AC_Freq: ";
unsigned int results[2];
void main(void)
{
	WDTCTL = WDTPW + WDTHOLD;  //关闭看门狗
	Dogs102x6_init();			//LCD初始化
	Dogs102x6_backlightInit(); 	//背光初始化
	Dogs102x6_setBacklight(6);	//设置LCD背光亮度
	Dogs102x6_clearScreen();			//清屏
//	P6DIR |= BIT0;
//	P6OUT |= BIT0;
//	P6DIR &= ~BIT5;
//	P6SEL |= BIT5;
//	ADC12MCTL0 = ADC12SREF_1+ADC12INCH_4;//参考电位通道的选择   输入通道选择
//	REFCTL0&=~REFMSTR;
//	ADC12CTL0=ADC12SHT02+ADC12ON+ADC12REFON;
//	ADC12CTL1 = ADC12SHP;
//	for(m = 0 ;m < 0x30 ; m++);
//	ADC12CTL0 |= ADC12ENC;
//	P6SEL |= BIT4;
//	_EINT();
//P6SEL = 0x50;
	ADC12CTL0 = ADC12ON + ADC12MSC + ADC12SHT0_2 + ADC12REFON + ADC12REF2_5V;
	ADC12CTL1 = ADC12SHP + ADC12CONSEQ_1;
	ADC12MCTL0 = ADC12INCH_4;
	ADC12MCTL1 = ADC12INCH_6;
	ADC12MCTL2 = ADC12INCH_2 + ADC12EOS;
	ADC12IE = 0x04;
	ADC12CTL0 |= ADC12ENC;
	P6SEL |= BIT4 + BIT6;
	_EINT();
//
//	P2DIR = 0x00;
//	P2IFG = 0x00;
//	P2IE = BIT2;
//	P2IES = 0xff;
//	P2IN = BIT2;
//	P2OUT = 0xff;
//	P2REN = 0xff;
//	__enable_interrupt();
//	P1DIR = 0x00;
//	P1IFG = 0x00;
//	P1IE = BIT7;
//	P1IES = 0xff;
//	P1IN = BIT7;
//	P1OUT |= BIT7;
//	P1REN |= BIT7;
//	__enable_interrupt();
//
	while(1)
	{
		//Dogs102x6_clearScreen();//清屏
		 ADC12CTL0|=ADC12SC;
		// __bis_SR_register(LPM4_bits + GIE);
		for(i=0;i<100;i++)
			{
	        	ADC12IE = 0x01;
				_delay_cycles(2000);
				sum_200+=results[0];
			}
		avrg = 2.5*sum_200/100/4096;
		if(avrg>=2.5)
		{
			ADC12CTL0 &= ~ADC12REFON;
			ADC12CTL0 &= ~ADC12REF2_5V;
			ADC12CTL0 |= ADC12REFON;
			ADC12CTL0|=ADC12SC;
			//__bis_SR_register(LPM4_bits + GIE);
			for(i=0;i<100;i++)
			{
				ADC12CTL0 |=ADC12SC;
				_delay_cycles(2000);
				sum_200 += results[1];
			}
		avrg = 1.5*sum_200/100/4096;
		}
		avrg *= 10000;
		num1 = (int)avrg;
		for(j=4;j>=0;j--)
			{
				rs[j] = '0'+num1%10;
				num1 /= 10;
			}
		key2_2 = key2 % 5;
		if(0 == key2_2)
		{
			Dogs102x6_stringDraw(0,4,"MULTIMETER",1);
			Dogs102x6_stringDraw(2,6,s1,0); 	//显示幅值字样
			Dogs102x6_stringDraw(3,6,s2,0);
			Dogs102x6_stringDraw(4,6,s3,0);
			Dogs102x6_stringDraw(6,6,s4,0);
		}
		else if(1 == key2_2)
		{
			for(k = 4 ; k >= 2 ; k--)
				{
					rs[k] = rs[k-1];
				}
			rs[1] = '.';
			rs[5] = 'V';
			rs[6] = '\0';
			Dogs102x6_stringDraw(0,4,"MULTIMETER",0);
			Dogs102x6_stringDraw(2,6,s1,1); 	//显示幅值字样
			Dogs102x6_stringDraw(3,6,s2,0);
			Dogs102x6_stringDraw(4,6,s3,0);
			Dogs102x6_stringDraw(6,6,s4,0);
			Dogs102x6_stringDraw(2,44,rs,1);
		}
		else if(2 == key2_2)
		{
			for(k = 4 ; k >= 2 ; k--)
			{
				rs[k] = rs[k-1];
			}
			rs[1] = '.';
			rs[5] = 'A';
			rs[6] = '\0';
			Dogs102x6_stringDraw(0,4,"MULTIMETER",0);
			Dogs102x6_stringDraw(2,6,s1,0); 	//显示幅值字样
			Dogs102x6_stringDraw(3,6,s2,1);
			Dogs102x6_stringDraw(4,6,s3,0);
			Dogs102x6_stringDraw(6,6,s4,0);
			Dogs102x6_stringDraw(3,44,rs,1); 	//显示读数
		}
		else if(3 == key2_2)
		{
			for(k = 4 ; k >= 2 ; k--)
			{
				rs[k] = rs[k-1];
			}
			rs[1] = '.';
			rs[5] = 'V';
			rs[6] = '\0';
			Dogs102x6_stringDraw(0,4,"MULTIMETER",0);
			Dogs102x6_stringDraw(2,6,s1,0); 	//显示幅值字样
			Dogs102x6_stringDraw(3,6,s2,0);
			Dogs102x6_stringDraw(4,6,s3,1);
			Dogs102x6_stringDraw(6,6,s4,0);
			Dogs102x6_stringDraw(4,44,rs,1); 	//显示读数
		}
		else if(4 == key2_2)
		{
			for(k = 4 ; k >= 2 ; k--)
			{
				rs[k] = rs[k-1];
			}
			rs[1] = '.';
			rs[5] = 'V';
			rs[6] = '\0';
			Dogs102x6_stringDraw(0,4,"MULTIMETER",0);
			Dogs102x6_stringDraw(2,6,s1,0); 	//显示幅值字样
			Dogs102x6_stringDraw(3,6,s2,0);
			Dogs102x6_stringDraw(4,6,s3,0);
			Dogs102x6_stringDraw(6,6,s4,1);  	//显示幅值字样
			Dogs102x6_stringDraw(6,44,rs,1); 	//显示读数
		}
	}
}
//#pragma vector = PORT2_VECTOR
//__interrupt void Port2_ISR(void)
//{
//	unsigned int temp;
//	int i;
//	for(i = 0 ; i < 12000 ; i++);
//	if((P2IN & 0xff) != 0xff)
//		temp = P2IFG;
//		P2IFG = 0x00;
//		if(0x04 == temp)
//			key2++;
//}
//
//
////#pragma vector = PORT1_VECTOR
////__interrupt void Port1_ISR(void)
////{
////	unsigned int temp;
////	int i;
////	for(i = 0 ; i < 12000 ; i++);
////	if((P1IN & 0xff) != 0xff)
////			temp = P1IFG;
////			P1IFG = 0x00;
////			if(0x80 == temp)
////				key1=1;
////			else
////				key1=0;
////}
//
#pragma vector = ADC12_VECTOR
__interrupt void ADC12ISR(void)
{
	results[0]=0;
	results[1]=0;
	switch(__even_in_range(ADC12IV,34))
	{
		case 0: break;
		case 2: break;
		case 4: break;
		case 6: break;
		case 8: break;
		case 10:
			results[0] = ADC12MEM0;
			results[1] = ADC12MEM1;
			//__bic_SR_register_on_exit(LPM4_bits);
	          break;
		case 12: break;
		case 14: break;
		case 16: break;
		case 18: break;
		case 20: break;
		case 22: break;
		case 24: break;
		case 26: break;
		case 28: break;
		case 30: break;
		case 32: break;
		case 34: break;
		default: break;
	}
ADC12IE = 0x00;
}

