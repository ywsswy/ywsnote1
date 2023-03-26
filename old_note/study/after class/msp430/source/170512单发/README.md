#include<msp430f5529.h>
char gr1 = 0;
void main() {
    WDTCTL = WDTPW | WDTHOLD;
    P3SEL = BIT3+BIT4;
	UCA0CTL1 |= UCSWRST;
	UCA0CTL1 |= UCSSEL_2;
	UCA0BR0 = 6;
	UCA0BR1 = 0;
	UCA0MCTL = UCBRS_0+UCBRF_13+UCOS16;
	UCA0CTL1 &= ~UCSWRST;
	_EINT();		//开启全局中断
	while(1){
		while(!(UCA0IFG & UCTXIFG));
		UCA0TXBUF = 's';
		__delay_cycles(3333);
	}
}
#pragma vector=USCI_A0_VECTOR
__interrupt void USCI_A0_ISR(void){
gr1 = 0;
}

