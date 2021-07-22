#include <msp430f5529.h>
/*
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	SFRIFG1 = 0;
	P1DIR |= BIT0;
	//P1DIR &=~BIT0;
	P1SEL |= BIT0;
	P2SEL |= BIT2;
	P2DIR |= BIT2;
	P7SEL |= BIT7;
	P7DIR |= BIT7;
	UCSCTL4 = UCSCTL4 &(~(SELA_7))|SELA_3;
	//UCSCTL4 = UCSCTL4 &(~(SELS_7|SELM_7|SELA_7))|SELS_2|SELM_2|SELA_0;
//	P8DIR |= BIT1;
//
//	TA0CCTL0 = CCIE;
//
//	TA0CCR0 = 500;
//	TA0CTL = TASSEL_0 + MC_1 + TACLR;
	//_EINT();
	while(1);
}
#pragma vector=TIMER0_A0_VECTOR
__interrupt void TIMERR0_A0_ISR(void)
{
	//P1OUT ^= BIT0;
	P8OUT ^= BIT1;
}*/
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	SFRIFG1 = 0;
	//P8DIR |= BIT1;
	P6DIR |= BIT7;
	//1
	//TA0CTL = TASSEL_2 + MC_2 + TAIE;//SMCLK,连续计数模式，使能TAIFG中断,~16hz
	//2
	TA0CCTL0 = CCIE;//CCR0中断使能
	//2.1
//	TA0CCR0 = 0xffff;
//	TA0CTL = TASSEL_2 + MC_1 + TACLR;//SMCLK,增计数模式，清除TAR计数器，//~16hz
	//3
	TA0CCR0 = 32767;
	TA0CTL = TASSEL_1 + MC_1 + TACLR;//ACLK,增计数模式，清除TAR计数器,//1hz
	_EINT();
	while(1);
}
#pragma vector=TIMER0_A0_VECTOR
__interrupt void TIMERR0_A0_ISR(void)
{
	P6OUT ^= BIT7;
}
/*
#pragma vector=TIMER0_A1_VECTOR
__interrupt void TIMERR0_A1_ISR(void)
{
	switch(__even_in_range(TA0IV,14))
	{
	case 0:break;//ccr1
	case 2:break;
	case 4:break;
	case 6:break;
	case 8:break;
	case 10:break;
	case 12:break;//ccr6
	case 14://TAIFG
		//P8DIR |= BIT1;
		P6OUT ^= BIT7;
		break;
	}
}*/

