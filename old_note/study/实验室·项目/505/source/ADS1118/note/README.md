//SCLK	DIN
//CS	DOUT
//GND	VDD
//AIN0	AIN3
//AIN1	AIN2
//M7.2signal in
#define MOSI_H     P7OUT|=BIT2
#define MOSI_L     P7OUT&=~BIT2
//M7.3signal out
#define MISO_H     P7OUT|=BIT3
#define MISO_L     P7OUT&=~BIT3
#define SCLK_H     P7OUT|=BIT1
#define SCLK_L     P7OUT&=~BIT1
#define CS_H       P7OUT|=BIT0
#define CS_L       P7OUT&=~BIT0
#define  MISO_IN 	P7DIR&=~BIT3
#define  MISO_OUT 	P7DIR|=BIT3
#define  READ_MISO  P7IN&BIT3
