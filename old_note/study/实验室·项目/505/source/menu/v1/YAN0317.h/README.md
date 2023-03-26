/*
 * YAN0317.h
 *
 *  Created on: 2017-3-17
 */
#ifndef YAN0317_H_
#define YAN0317_H_
#include <stdint.h>
//change
#include "HAL_Dogs102x6.h"
#define contrastSetpointAddress     0x1880
#define LED145678_PORT_DIR          P1DIR
#define LED145678_PORT_OUT          P1OUT
#define LED23_PORT_DIR              P8DIR
#define LED23_PORT_OUT              P8OUT
#define LED1        0x01
#define LED2        0x02
#define LED3        0x04
#define LED4        0x08
#define LED5        0x10
#define LED6        0x20
#define LED7        0x40
#define LED8        0x80
#define LED_ALL     0xFF
#define BUTTON_S2       0x0400
#define BUTTON_S1       0x0080
#define BUTTON_ALL      0x0480
#define BUTTON_PORT_DIR   PADIR
#define BUTTON_PORT_OUT   PAOUT
#define BUTTON_PORT_SEL   PASEL
#define BUTTON_PORT_REN   PAREN
#define BUTTON_PORT_IE    PAIE
#define BUTTON_PORT_IES   PAIES
#define BUTTON_PORT_IFG   PAIFG
#define BUTTON_PORT_IN    PAIN
#define BUTTON1_PIN       BIT7       //P1.7
#define BUTTON2_PIN       BIT2       //P2.2
#define BUTTON1_IFG       P1IFG      //P1.7
#define BUTTON2_IFG       P2IFG      //P1.7
#define WHEEL_PORT_DIR P8DIR
#define WHEEL_PORT_OUT P8OUT
#define WHEEL_ENABLE  BIT0
#define ADC_PORT_SEL  P6SEL
#define ADC_INPUT_A5  BIT5
extern volatile uint16_t buttonsPressed;
extern void yItoa(int num,char*str,int radix);
extern void yBoard_init(void);
extern void yBoard_ledOn(uint8_t ledMask);
extern void yBoard_ledOff(uint8_t ledMask);
extern void yBoard_ledToggle(uint8_t ledMask);
extern void yButtons_init(uint16_t buttonsMask);
extern void yButtons_interruptEnable(uint16_t buttonsMask);
extern void yButtons_interruptDisable(uint16_t buttonsMask);
extern uint8_t yMenu_active(char **menuText);
extern void yWheel_init(void);
extern uint8_t yWheel_getPosition(void);
extern uint16_t yWheel_getValue(void);
extern void yWheel_disable(void);
extern void yWheel_enable(void);
#endif /* YAN0317_H_ */

