#define  uchar unsigned char
#define  uint  unsigned int
#define  ulong unsigned long
#define CPU_F ((double)8000000)
#define delay_us(x) __delay_cycles((long)(CPU_F*(double)x/1000000.0))
#define delay_ms(x) __delay_cycles((long)(CPU_F*(double)x/1000.0))
#define CONFIG_VALUE    0x40ea //0X038B       //AIN0-AIN1  4.096  128sps  pull on DOUT
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
extern void ADS1118_Init(void);
extern int ADS_SEL_Read(uchar CH1,uchar CH2,uchar Ref) ;
extern uint Write_ADS1118(uint dat,uchar mode);
//SCLK	DIN
//CS	DOUT
//GND	VDD
//AIN0	AIN3
//AIN1	AIN2

