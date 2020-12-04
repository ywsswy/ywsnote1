/*
 * YAN0317.c
 *
 *  Created on: 2017-3-17
 */
#include "msp430.h"
#include "YAN0317.h"
#include "YAN0317VAR.h"
uint16_t positionData;
uint16_t positionDataOld;
volatile uint16_t buttonsPressed = 0;
void yItoa(int num,char*str,int radix){
	char index[]="0123456789ABCDEF",temp;
	unsigned unum;
	int i=0,j,k;
	if(radix==10&&num<0){
		unum=(unsigned)-num;
		str[i++]='-';
		k=1;
	}
	else {
		unum=(unsigned)num;
		k=0;
	}
	do{
		str[i++]=index[unum%(unsigned)radix];
		unum/=radix;
	}while(unum);
	str[i]='\0';
	for(j=k;j<=(i-1)/2;j++){
		temp=str[j];
		str[j]=str[i-1+k-j];
		str[i-1+k-j]=temp;
}}
//button led
void yBoard_init(void)
{
    // Configure button ports
    PADIR &= ~0x0480;               // Buttons on P1.7/P2.2 are inputs
    // Configure LED ports
    LED145678_PORT_OUT &= ~(BIT0 + BIT1 + BIT2 + BIT3 + BIT4 + BIT5);
    LED145678_PORT_DIR |= BIT0 + BIT1 + BIT2 + BIT3 + BIT4 + BIT5;
    LED23_PORT_OUT &= ~(BIT1 + BIT2);
    LED23_PORT_DIR |= BIT1 + BIT2;
    // Configure Wheel ports
    P6DIR &= ~BIT5;                 // A5 ADC input
    P8OUT &= ~BIT0;                 // POT_PWR
    P8DIR |= BIT0;
    // Configure Dogs102x6 ports
    P5OUT &= ~(BIT6 + BIT7);        // LCD_C/D, LCD_RST
    P5DIR |= BIT6 + BIT7;
    P7OUT &= ~(BIT4 + BIT6);        // LCD_CS, LCD_BL_EN
    P7DIR |= BIT4 + BIT6;
    P4OUT &= ~(BIT1 + BIT3);        // SIMO, SCK
    P4DIR &= ~BIT2;                 // SOMI pin is input
    P4DIR |= BIT1 + BIT3;
}
void yBoard_ledOn(uint8_t ledMask)
{
    if (ledMask & LED1) LED145678_PORT_OUT |= BIT0;
    if (ledMask & LED2) LED23_PORT_OUT |= BIT1;
    if (ledMask & LED3) LED23_PORT_OUT |= BIT2;
    if (ledMask & LED4) LED145678_PORT_OUT |= BIT1;
    if (ledMask & LED5) LED145678_PORT_OUT |= BIT2;
    if (ledMask & LED6) LED145678_PORT_OUT |= BIT3;
    if (ledMask & LED7) LED145678_PORT_OUT |= BIT4;
    if (ledMask & LED8) LED145678_PORT_OUT |= BIT5;
}
/***************************************************************************//**
 * @brief  Turn off LEDs
 * @param  ledMask   Use values defined in HAL_board.h for the LEDs to turn off
 * @return none
 ******************************************************************************/
void yBoard_ledOff(uint8_t ledMask)
{
    if (ledMask & LED1) LED145678_PORT_OUT &= ~BIT0;
    if (ledMask & LED2) LED23_PORT_OUT &= ~BIT1;
    if (ledMask & LED3) LED23_PORT_OUT &= ~BIT2;
    if (ledMask & LED4) LED145678_PORT_OUT &= ~BIT1;
    if (ledMask & LED5) LED145678_PORT_OUT &= ~BIT2;
    if (ledMask & LED6) LED145678_PORT_OUT &= ~BIT3;
    if (ledMask & LED7) LED145678_PORT_OUT &= ~BIT4;
    if (ledMask & LED8) LED145678_PORT_OUT &= ~BIT5;
}
//开关 LEDs
void yBoard_ledToggle(uint8_t ledMask)
{
    if (ledMask & LED1) LED145678_PORT_OUT ^= BIT0;
    if (ledMask & LED2) LED23_PORT_OUT ^= BIT1;
    if (ledMask & LED3) LED23_PORT_OUT ^= BIT2;
    if (ledMask & LED4) LED145678_PORT_OUT ^= BIT1;
    if (ledMask & LED5) LED145678_PORT_OUT ^= BIT2;
    if (ledMask & LED6) LED145678_PORT_OUT ^= BIT3;
    if (ledMask & LED7) LED145678_PORT_OUT ^= BIT4;
    if (ledMask & LED8) LED145678_PORT_OUT ^= BIT5;
}
void yButtons_init(uint16_t buttonsMask)
{
    BUTTON_PORT_OUT |= buttonsMask;  //buttons are active low
    BUTTON_PORT_REN |= buttonsMask;  //pullup resistor
    BUTTON_PORT_SEL &= ~buttonsMask; //
}
void yButtons_interruptEnable(uint16_t buttonsMask)
{
    BUTTON_PORT_IES &= ~buttonsMask; //select falling edge trigger
    BUTTON_PORT_IFG &= ~buttonsMask; //clear flags
    BUTTON_PORT_IE |= buttonsMask;   //enable interrupts
}
void yButtons_interruptDisable(uint16_t buttonsMask)
{
    BUTTON_PORT_IE &= ~buttonsMask;
}
uint8_t yWheel_getPosition(void)
{
    uint8_t position = 0;
    yWheel_getValue();
    //determine which position the wheel is in
//    if (positionData > 0x0806)//2054=260*8
//        position = 7 - (positionData - 0x0806) / 260;  //scale the data for 8 different positions
//    else
//        position = positionData / 260;
    position = positionData / (4108/Ytotalitem);
    return position;
}
uint8_t yMenu_active(char **menuText)
{
    uint8_t i;
    uint8_t position = 0;
    int lastPosition = -1;
    Dogs102x6_clearScreen();
    Dogs102x6_stringDraw(0, 0, menuText[0], DOGS102x6_DRAW_NORMAL); // Print the title
    buttonsPressed = 0;
    while (!(buttonsPressed & BUTTON_S1))
                                                                    // is made
    {
        position = yWheel_getPosition();
        if (position != lastPosition)                               // Update position if it is
                                                                    // changed
        {
        	if(position+1 > Ynowpagefirstitem+Yonepageitems-1)
        	{
        		Ynowpagefirstitem = position+1-Yonepageitems+1;
        	}
        	else if(position+1 < Ynowpagefirstitem)
        	{
        		Ynowpagefirstitem = position+1;
        	}
        	for (i = 1; i <= Yonepageitems; i++)                      // Display menu items
			{
    			if (i-1+Ynowpagefirstitem != position+1){
					Dogs102x6_stringDraw(i, 0, menuText[i-1+Ynowpagefirstitem], DOGS102x6_DRAW_NORMAL);
				}
				else {
					// Highlight item at current position
					Dogs102x6_stringDraw(i, 0, menuText[i-1+Ynowpagefirstitem], DOGS102x6_DRAW_INVERT);
				}
			}
            lastPosition = position;
        }
    }
    return position+1;
}
void yWheel_disable(void)
{
    WHEEL_PORT_OUT &= ~WHEEL_ENABLE;                   //disable wheel
    ADC12CTL0 &= ~ADC12ENC;                            // Disable conversions
    ADC12CTL0 &= ~ADC12ON;                             // ADC12 off
}
void yWheel_enable(void)
{
    WHEEL_PORT_OUT |= WHEEL_ENABLE;                    //disable wheel
    ADC12CTL0 |= ADC12ON;                              // ADC12 on
    ADC12CTL0 |= ADC12ENC;                             // Enable conversions
}
void yWheel_init(void)
{
    WHEEL_PORT_DIR |= WHEEL_ENABLE;						//8.0出高
    WHEEL_PORT_OUT |= WHEEL_ENABLE;
    ADC12CTL0 = ADC12SHT02 + ADC12ON;                  // Sampling time, ADC12 on
    ADC12CTL1 = ADC12SHP;                              // Use sampling timer
    											//采样保持信号（SAMPCON）来源选择
    											//0来自SHI信号
    											//1来自采样定时器（SHI信号上升沿触发）
    ADC12MCTL0 = ADC12INCH_5;                          // Use A5 (wheel) as input
    ADC12CTL0 |= ADC12ENC;                             // Enable conversions
    ADC_PORT_SEL |= ADC_INPUT_A5;                      // P6.5 ADC option select (A5)
}
uint16_t yWheel_getValue(void)
{
    //measure ADC value
    ADC12IE = 0x01;                                    // Enable interrupt
    ADC12CTL0 |= ADC12SC;                              // Start sampling/conversion
    __bis_SR_register(LPM0_bits + GIE);                // LPM0, ADC12_ISR will force exit
    ADC12IE = 0x00;                                    // Disable interrupt
    //add hysteresis on wheel to remove fluctuations
    if (positionData > positionDataOld)
        if ((positionData - positionDataOld) > 10)
            positionDataOld = positionData;            //use new data if change is beyond
                                                       // fluctuation threshold
        else
            positionData = positionDataOld;            //use old data if change is not beyond
                                                       // fluctuation threshold
    else if ((positionDataOld - positionData) > 10)
        positionDataOld = positionData;                //use new data if change is beyond
                                                       // fluctuation threshold
    else
        positionData = positionDataOld;                //use old data if change is not beyond
                                                       // fluctuation threshold
    return positionData;
}
#pragma vector = ADC12_VECTOR
__interrupt void ADC12_ISR(void)
{
    switch (__even_in_range(ADC12IV, ADC12IV_ADC12IFG15))
    {
        // Vector  ADC12IV_NONE:  No interrupt
        case  ADC12IV_NONE:
            break;
        // Vector  ADC12IV_ADC12OVIFG:  ADC overflow
        case  ADC12IV_ADC12OVIFG:
            break;
        // Vector  ADC12IV_ADC12TOVIFG:  ADC timing overflow
        case  ADC12IV_ADC12TOVIFG:
            break;
        // Vector  ADC12IV_ADC12IFG0: ADC12IFG0:
        case  ADC12IV_ADC12IFG0:
            positionData = ADC12MEM0;                  // ADC12MEM = A0 > 0.5AVcc?
            __bic_SR_register_on_exit(LPM0_bits);      // Exit active CPU
            break;
        // Vector  ADC12IV_ADC12IFG1:  ADC12IFG1
        case  ADC12IV_ADC12IFG1:
            break;
        // Vector ADC12IV_ADC12IFG2:  ADC12IFG2
        case ADC12IV_ADC12IFG2:
            break;
        // Vector ADC12IV_ADC12IFG3:  ADC12IFG3
        case ADC12IV_ADC12IFG3:
            break;
        // Vector ADC12IV_ADC12IFG4:  ADC12IFG4
        case ADC12IV_ADC12IFG4:
            break;
        // Vector ADC12IV_ADC12IFG5:  ADC12IFG5
        case ADC12IV_ADC12IFG5:
            break;
        // Vector ADC12IV_ADC12IFG6:  ADC12IFG6
        case ADC12IV_ADC12IFG6:
            break;
        // Vector ADC12IV_ADC12IFG7:  ADC12IFG7
        case ADC12IV_ADC12IFG7:
            break;
        // Vector ADC12IV_ADC12IFG8:  ADC12IFG8
        case ADC12IV_ADC12IFG8:
            break;
        // Vector ADC12IV_ADC12IFG9:  ADC12IFG9
        case ADC12IV_ADC12IFG9:
            break;
        // Vector ADC12IV_ADC12IFG10:  ADC12IFG10
        case ADC12IV_ADC12IFG10:
            break;
        // Vector ADC12IV_ADC12IFG11:  ADC12IFG11
        case ADC12IV_ADC12IFG11:
            break;
        // Vector ADC12IV_ADC12IFG12:  ADC12IFG12
        case ADC12IV_ADC12IFG12:
            break;
        // Vector ADC12IV_ADC12IFG13:  ADC12IFG13
        case ADC12IV_ADC12IFG13:
            break;
        // Vector ADC12IV_ADC12IFG14:  ADC12IFG14
        case ADC12IV_ADC12IFG14:
            break;
        // Vector ADC12IV_ADC12IFG15:  ADC12IFG15
        case ADC12IV_ADC12IFG15:
            break;
        default:
            break;
    }
}
#pragma vector=PORT2_VECTOR
__interrupt void Port2_ISR(void)
{
    //
    // Context save interrupt flag before calling interrupt vector.
    // Reading interrupt vector generator will automatically clear IFG flag
    //
    buttonsPressed = PAIFG & BUTTON_ALL;
    switch (__even_in_range(P2IV, P2IV_P2IFG7))
    {
        // Vector  P2IV_NONE:  No Interrupt pending
        case  P2IV_NONE:
            break;
        // Vector  P2IV_P2IFG0:  P2IV P2IFG.0
        case  P2IV_P2IFG0:
            break;
        // Vector  P2IV_P2IFG1:  P2IV P2IFG.1
        case  P2IV_P2IFG1:
            break;
        // Vector  P2IV_P2IFG2:  P2IV P2IFG.2
        case  P2IV_P2IFG2:
            break;
        // Vector  P2IV_P2IFG3:  P2IV P2IFG.3
        case  P2IV_P2IFG3:
            break;
        // Vector  P2IV_P2IFG4:  P2IV P2IFG.4
        case  P2IV_P2IFG4:
            break;
        // Vector  P2IV_P2IFG5:  P2IV P2IFG.5
        case  P2IV_P2IFG5:
            break;
        // Vector  P2IV_P2IFG1:  P2IV P2IFG.6
        case  P2IV_P2IFG6:
            break;
        // Vector  P2IV_P2IFG7:  P2IV P2IFG.7
        case  P2IV_P2IFG7:
            break;
        // Default case
        default:
            break;
    }
}
#pragma vector=PORT1_VECTOR
__interrupt void Port1_ISR(void)
{
    //
    // Context save interrupt flag before calling interrupt vector.
    // Reading interrupt vector generator will automatically clear IFG flag
    //
    buttonsPressed = PAIFG & BUTTON_ALL;
    switch (__even_in_range(P1IV, P1IV_P1IFG7))
    {
        // Vector  P1IV_NONE:  No Interrupt pending
        case  P1IV_NONE:
            break;
        // Vector  P1IV_P1IFG0:  P1IV P1IFG.0
        case  P1IV_P1IFG0:
            break;
        // Vector  P1IV_P1IFG1:  P1IV P1IFG.1
        case  P1IV_P1IFG1:
            break;
        // Vector  P1IV_P1IFG2:  P1IV P1IFG.2
        case  P1IV_P1IFG2:
            break;
        // Vector  P1IV_P1IFG3:  P1IV P1IFG.3
        case  P1IV_P1IFG3:
            break;
        // Vector  P1IV_P1IFG4:  P1IV P1IFG.4
        case  P1IV_P1IFG4:
            break;
        // Vector  P1IV_P1IFG5:  P1IV P1IFG.5
        case  P1IV_P1IFG5:
            break;
        // Vector  P1IV_P1IFG1:  P1IV P1IFG.6
        case  P1IV_P1IFG6:
            break;
        // Vector  P1IV_P1IFG7:  P1IV P1IFG.7
        case  P1IV_P1IFG7:
            break;
        // Default case
        default:
            break;
    }
}

