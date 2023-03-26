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
	while(1);
}
#pragma vector=USCI_A1_VECTOR
__interrupt void USCI_A1_ISR(void){
	switch(__even_in_range(UCA1IV,4)){
	case 2:
		while(!(UCA1IFG&UCTXIFG));
		UCA1TXBUF = UCA1RXBUF+1;
		//UCA1TXBUF = 13;
		break;
	default:
		break;
	}
}
/*USB转TTL供电3V3V
允许430的3.3给蓝牙供电
P4.4	接下载器的RXD
P4.5	接下载器的TXD
供电口拨到最下面，不是中间*/
