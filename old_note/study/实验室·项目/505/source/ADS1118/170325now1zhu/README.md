    P7DIR|=BIT3+BIT2+BIT1;
    delay_us(1);
    P3DIR&=~BIT2;
#define CONFIG_VALUE    0x40ea //0X038B       //AIN0-AIN1  4.096  128sps  pull on DOUT
//M7.2signal in
#define MOSI_H     P7OUT|=BIT1
#define MOSI_L     P7OUT&=~BIT1
//M7.3signal out
#define CS_H     P7OUT|=BIT3
#define CS_L     P7OUT&=~BIT3
#define SCLK_H     P7OUT|=BIT2
#define SCLK_L     P7OUT&=~BIT2
//#define CS_H       P7OUT|=BIT0
//#define CS_L       P7OUT&=~BIT0
#define MISO_H		P3OUT|=BIT2
#define MISO_L		P3OUT&=~BIT2
#define  MISO_IN 	P3DIR&=~BIT2
#define  MISO_OUT 	P3DIR|=BIT2
#define  READ_MISO  P3IN&BIT2
