#include <msp430f5529.h>
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	P4SEL |= BIT4+BIT5;
	UCA1CTL1 |= UCSWRST;
	UCA1CTL1 |= UCSSEL_2;
	UCA1BR0 = 6;
	UCA1BR1 = 0;
	UCA1MCTL = UCBRS_0+UCBRF_13+UCOS16;
	UCA1CTL1 &= ~UCSWRST;
	UCA1IE |= UCRXIE;
	
	_EINT();
	while(1){
		while(!(UCA1IFG&UCTXIFG));
		UCA1TXBUF = 0xF0;
	}
}
#pragma vector=USCI_A1_VECTOR
__interrupt void USCI_A1_ISR(void){
}
/*USB转TTL供电3V3V
P4.4	接下载器的RXD
P4.5	接下载器的TXD
供电口拨到最下面，不是中间*/
//a2
//8+2,2
//1010 0010
//13
//0001 0011

