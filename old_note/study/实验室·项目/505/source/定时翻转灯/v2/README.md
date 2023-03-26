#include <msp430f5529.h>
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	SFRIFG1 = 0;
	P6DIR |= BIT7;
	TA0CTL = TASSEL_2 + MC_2 + TAIE;//SMCLK,增计数模式，使能TAIFG中断
	_EINT();
	while(1);
}
#pragma vector=TIMER0_A1_VECTOR
__interrupt void TIMERR0_A1_ISR(void)
{
	switch(__even_in_range(TA0IV,14))
	{
	case 0:break;
	case 2:break;
	case 4:break;
	case 6:break;
	case 8:break;
	case 10:break;
	case 12:break;
	case 14:
		P6OUT ^= BIT7;
		break;
	}
}

