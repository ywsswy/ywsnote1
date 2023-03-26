#include<msp430f5529.h>
int main(void) {
    WDTCTL = WDTPW | WDTHOLD;
    P3SEL = BIT3 + BIT4;
	UCA0CTL1 |= UCSWRST;
	UCA0CTL1 |= UCSSEL_2;
	UCA0BR0 = 6;
	UCA0BR1 = 0;
	UCA0MCTL = UCBRS_0+UCBRF_13+UCOS16;
	UCA0CTL1 &= ~UCSWRST;
	UCA0IE |= UCRXIE;
	_EINT();		//开启全局中断
	while(1){
	}
	return 0;
}
#pragma vector=USCI_A0_VECTOR
__interrupt void USCI_A0_ISR(void){
	switch(__even_in_range(UCA0IV,4)){
	case 2://接收中断
		while(!(UCA0IFG&UCTXIFG));
		UCA0TXBUF = UCA0RXBUF;
		break;
	case 4:
		break;
	default:
		break;
	}
}

