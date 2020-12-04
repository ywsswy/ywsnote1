#include<msp430f5529.h>
int main(void) {
    WDTCTL = WDTPW | WDTHOLD;
    P4SEL = BIT4+BIT5;
	UCA1CTL1 |= UCSWRST;
	UCA1CTL1 |= UCSSEL_2;
	UCA1BR0 = 6;
	UCA1BR1 = 0;
	UCA1MCTL = UCBRS_0+UCBRF_13+UCOS16;
	UCA1CTL1 &= ~UCSWRST;
	UCA1IE |= UCRXIE;
	_EINT();		//开启全局中断
	while(1){
	}
	return 0;
}
#pragma vector=USCI_A1_VECTOR
__interrupt void USCI_A1_ISR(void){
	switch(__even_in_range(UCA1IV,4)){
	case 2://接收中断
		while(!(UCA1IFG&UCTXIFG));
		UCA1TXBUF = UCA1RXBUF;
		break;
	case 4:
		break;
	default:
		break;
	}
}

