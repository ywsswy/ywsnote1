#include <msp430f5529.h>
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	SFRIFG1 = 0;
	P1DIR &=~BIT0;
	P1SEL |= BIT0;//P1.0的第二功能：TA0CLK输入
	P8DIR |= BIT1;//led2
	TA0CCTL0 = CCIE;
	TA0CCR0 = 500;//如果输入为500hz，则led2每秒翻转1次
	TA0CTL = TASSEL_0 + MC_1 + TACLR;//TA0时钟源选择外部引脚输入，增计数模式
	_EINT();
	while(1);
}
#pragma vector=TIMER0_A0_VECTOR
__interrupt void TIMERR0_A0_ISR(void)
{
	P8OUT ^= BIT1;
}

