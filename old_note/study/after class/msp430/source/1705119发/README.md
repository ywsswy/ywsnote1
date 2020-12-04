#include <msp430f5529.h>
void main(void)
{
  WDTCTL = WDTPW + WDTHOLD;                 // Stop WDT
  P3SEL = BIT3+BIT4;                        // P3.4,5 = USCI_A0 TXD/RXD
  UCA0CTL1 |= UCSWRST;                      // **Put state machine in reset**
  UCA0CTL1 |= UCSSEL_2;                     // SMCLK
  UCA0BR0 = 6;                              // 1MHz 9600 (see User's Guide)
  UCA0BR1 = 0;                              // 1MHz 9600
  UCA0MCTL = UCBRS_0 + UCBRF_13 + UCOS16;   // Modln UCBRSx=0, UCBRFx=0,
                                            // over sampling
  UCA0CTL1 &= ~UCSWRST;
	while(1){                   // **Initialize USCI state machine**
		_EINT();
		while (!(UCA0IFG&UCTXIFG));             // USCI_A0 TX buffer ready?
			UCA0TXBUF = 's';
	}
}
#pragma vector=USCI_A0_VECTOR
__interrupt void USCI_A0_ISR(void)
{
}

