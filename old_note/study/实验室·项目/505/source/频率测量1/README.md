//P10->TA0CLK输入
#include <msp430f5529.h>
#include "HAL_PMM.h"
#include "HAL_Dogs102x6.h"
#include "lab1.h"
#include "stdint.h"
#include "stdio.h"
#include "lab1.h"
#include "YAN0317.h"
#include "YAN0317VAR.h"
#include "msp430.h"
int yhh;
int yhf;
int yhg;
float yff = 0.0;
char strhh[30] = "";
void main(void) {
    uint8_t contrast = *((unsigned char *)contrastSetpointAddress);
	WDTCTL = WDTPW + WDTHOLD;
	SetVCore(3);                                  //设VCore
	SFRIFG1 = 0;
	yBoard_init();
/////////////////////////////////////////////////////////////
    Dogs102x6_init();                            //初始化LCD// Contrast not programed in Flash Yet
    contrast = 11;
    Dogs102x6_setContrast(contrast);             //设置初始对比度值
    Dogs102x6_clearScreen();                     //清屏
	P1DIR &=~BIT0;
	P1SEL |= BIT0;//P1.0的第二功能：TA0CLK输入
	P8DIR |= BIT1;//led2
	P8DIR |= BIT2;//led3
	TA1CCTL0 = CCIE;//CCR0中断使能
	TA1CCR0 = 32767;
	TA1CTL = TASSEL_1 + MC_1 + TACLR;//ACLK,增计数模式，清除TAR计数器,//1hz
	TA0CCTL0 = CCIE;
	TA0CCR0 = 0xffff;
	TA0CTL = TASSEL_0 + MC_1 + TACLR;//TA0时钟源选择外部引脚输入，增计数模式
	_EINT();
	while(1){
		yhg = yhf;
		yff = (float)yhg*1.0252-0.2733;
		//yItoa(yhg,strhh,10);
		sprintf(strhh,"%f",yff);
	    Dogs102x6_stringDraw(3, 0, strhh, DOGS102x6_DRAW_NORMAL);
	}
}
#pragma vector=TIMER1_A0_VECTOR
__interrupt void TIMERR1_A0_ISR(void)
{
	//P8OUT ^= BIT2;
//	yhg=yhh;
//	yhh=0;
	TA0CTL &= ~MC_0;
	TA1CTL &= ~MC_0;
	yhf = TA0R;
	TA0CTL |= TACLR;
	TA0CTL |= MC_1;
	TA1CTL |= MC_1;
}
#pragma vector=TIMER0_A0_VECTOR
__interrupt void TIMERR0_A0_ISR(void)
{
	//_NOP();
	//P8OUT ^= BIT1;
	//yhh++;
}

