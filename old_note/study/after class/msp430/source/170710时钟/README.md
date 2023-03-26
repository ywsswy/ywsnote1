#include <msp430f5529.h>
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	SFRIFG1 = 0;
	P7DIR |= BIT1;
	P7DIR |= BIT2;
	TA1CCTL0 = CCIE;//CCR0中断使能
	TA1CCR0 = 13;
	TA1CTL = TASSEL_2 + MC_1 + TACLR;//ACLK,增计数模式，清除TAR计数器,//1hz
	TA0CCTL0 = CCIE;//CCR0中断使能
	TA0CCR0 = 524;
	TA0CTL = TASSEL_2 + MC_1 + TACLR;//SMCLK,连续计数模式，使能TAIFG中断,~16hz
	_EINT();
	while(1);
}
#pragma vector=TIMER1_A0_VECTOR
__interrupt void TIMERR1_A0_ISR(void)
{
	P7OUT ^= BIT1;
}
#pragma vector=TIMER0_A0_VECTOR
__interrupt void TIMERR0_A0_ISR(void)
{
	P7OUT ^= BIT2;
}

