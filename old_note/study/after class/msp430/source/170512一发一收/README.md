#include<msp430f5529.h>
#include"oled.h"
char gr1 = '3';
void main(void) {
    WDTCTL = WDTPW | WDTHOLD;
    OLED_Init();
    P3SEL = BIT3 + BIT4;
	UCA0CTL1 |= UCSWRST;
	UCA0CTL1 |= UCSSEL_2;
	UCA0BR0 = 6;
	UCA0BR1 = 0;
	UCA0MCTL = UCBRS_0+UCBRF_13+UCOS16;
	UCA0CTL1 &= ~UCSWRST;
	OLED_ShowChar(0,0,gr1);
	UCA0IE |= UCRXIE;
    P4SEL = BIT4 + BIT5;
	UCA1CTL1 |= UCSWRST;
	UCA1CTL1 |= UCSSEL_2;
	UCA1BR0 = 6;
	UCA1BR1 = 0;
	UCA1MCTL = UCBRS_0+UCBRF_13+UCOS16;
	UCA1CTL1 &= ~UCSWRST;
	_EINT();		//开启全局中断
	while(1){
		OLED_ShowChar(0,0,gr1);
		while(!(UCA1IFG & UCTXIFG));
		UCA1TXBUF = 'x';
		__delay_cycles(3333);
	}
}
#pragma vector=USCI_A0_VECTOR
__interrupt void USCI_A0_ISR(void){
	switch(__even_in_range(UCA0IV,4)){
	case 2://接收中断
		while(!(UCA0IFG&UCTXIFG));
		gr1 = UCA0RXBUF;
		UCA0TXBUF = 0x33;
		break;
	case 4:
		break;
	default:
		break;
	}
}
#pragma vector=USCI_A1_VECTOR
__interrupt void USCI_A1_ISR(void){
}

