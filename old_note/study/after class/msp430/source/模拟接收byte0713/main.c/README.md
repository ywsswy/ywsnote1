//MSP-EXP430F5529 白板子框架示例程序
#include "msp430f5529.h"
#include "Yinit.h"			//包含各种初始化
#include "Yuser.h"			//【提供用户修改及使用，用户仅修改Yuser.c文件即可】
							//【用户仅需修改Yuser.c文件即可实现基础功能类的程序！！】
void SetVcoreUp (unsigned int level);
void main(){
	  volatile unsigned int i;
	  WDTCTL = WDTPW+WDTHOLD;                       // P1.1 output
	  P1DIR |= BIT0;                            // ACLK set out to pins
	  P1SEL |= BIT0;
	  P2DIR |= BIT2;                            // SMCLK set out to pins
	  P2SEL |= BIT2;
	  P7DIR |= BIT7;                            // MCLK set out to pins
	  P7SEL |= BIT7;
	  // Increase Vcore setting to level3 to support fsystem=25MHz
	  // NOTE: Change core voltage one level at a time..
	  SetVcoreUp (0x01);
	  SetVcoreUp (0x02);
	  SetVcoreUp (0x03);
	  UCSCTL3 = SELREF_2;                       // Set DCO FLL reference = REFO
	  UCSCTL4 |= SELA_2;                        // Set ACLK = REFO
	  __bis_SR_register(SCG0);                  // Disable the FLL control loop
	  UCSCTL0 = 0x0000;                         // Set lowest possible DCOx, MODx
	  UCSCTL1 = DCORSEL_7;                      // Select DCO range 50MHz operation
	  UCSCTL2 = FLLD_1 + 762;                   // Set DCO Multiplier for 25MHz
	                                            // (N + 1) * FLLRef = Fdco
	                                            // (762 + 1) * 32768 = 25MHz
	                                            // Set FLL Div = fDCOCLK/2
	  __bic_SR_register(SCG0);                  // Enable the FLL control loop
	  // Worst-case settling time for the DCO when the DCO range bits have been
	  // changed is n x 32 x 32 x f_MCLK / f_FLL_reference. See UCS chapter in 5xx
	  // UG for optimization.
	  // 32 x 32 x 25 MHz / 32,768 Hz ~ 780k MCLK cycles for DCO to settle
	  __delay_cycles(782000);
	  // Loop until XT1,XT2 & DCO stabilizes - In this case only DCO has to stabilize
	  do
	  {
	    UCSCTL7 &= ~(XT2OFFG + XT1LFOFFG + DCOFFG);
	                                            // Clear XT2,XT1,DCO fault flags
	    SFRIFG1 &= ~OFIFG;                      // Clear fault flags
	  }while (SFRIFG1&OFIFG);                   // Test oscillator fault flag
	  P2DIR |= BIT4;
	  P2SEL |= BIT4;
	  TA2CCR0 = 320;
	  TA2CCR1 = 160;
	  TA2CCTL1 = OUTMOD_6;
	  TA2CTL = TASSEL_2 + MC_3 + TACLR;
	yInit();		//各模块初始化
	SFRIFG1 = 0;	//清空中断标志
	_EINT();		//开启全局中断
	yUserStart();	//启动用户主程序
}
void SetVcoreUp (unsigned int level)
{
  // Open PMM registers for write
  PMMCTL0_H = PMMPW_H;
  // Set SVS/SVM high side new level
  SVSMHCTL = SVSHE + SVSHRVL0 * level + SVMHE + SVSMHRRL0 * level;
  // Set SVM low side to new level
  SVSMLCTL = SVSLE + SVMLE + SVSMLRRL0 * level;
  // Wait till SVM is settled
  while ((PMMIFG & SVSMLDLYIFG) == 0);
  // Clear already set flags
  PMMIFG &= ~(SVMLVLRIFG + SVMLIFG);
  // Set VCore to new level
  PMMCTL0_L = PMMCOREV0 * level;
  // Wait till new level reached
  if ((PMMIFG & SVMLIFG))
    while ((PMMIFG & SVMLVLRIFG) == 0);
  // Set SVS/SVM low side to new level
  SVSMLCTL = SVSLE + SVSLRVL0 * level + SVMLE + SVSMLRRL0 * level;
  // Lock PMM registers for write access
  PMMCTL0_H = 0x00;
}

