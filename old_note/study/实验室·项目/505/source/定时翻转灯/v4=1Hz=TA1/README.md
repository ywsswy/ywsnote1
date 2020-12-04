#include <msp430f5529.h>
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	SFRIFG1 = 0;
	P8DIR |= BIT1;
	TA1CCTL0 = CCIE;//CCR0中断使能
	TA1CCR0 = 32767;
	TA1CTL = TASSEL_1 + MC_1 + TACLR;//ACLK,增计数模式，清除TAR计数器,//1hz
	_EINT();
	while(1);
}
#pragma vector=TIMER1_A0_VECTOR
__interrupt void TIMERR1_A0_ISR(void)
{
	P8OUT ^= BIT1;
}

