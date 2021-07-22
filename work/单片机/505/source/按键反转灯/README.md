#include <msp430f5529.h>
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	SFRIFG1 = 0;//清中断
	P8DIR |= BIT1;//led2
	P1REN |= BIT7;
	P1OUT |= BIT7;//上拉
	P1IES |= BIT7;//下降沿触发
	
	P1IE |= BIT7;//使能
	_EINT();//全局使能
	while(1);
}
#pragma vector = PORT1_VECTOR
__interrupt void Port_1(void)
{
	P8OUT ^= BIT1;
	P1IFG &= ~BIT7;//！
}
