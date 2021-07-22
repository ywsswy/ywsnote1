#include <msp430f5529.h>
#include "ADS1118.h"
void ADS1118_Init(void)
{
    P7DIR|=BIT0+BIT1+BIT2;
    delay_us(1);
    P7DIR&=~BIT3;
    delay_us(1);
	CS_H;
	delay_us(1);
	SCLK_H;
	delay_us(1);
	MOSI_L;
	delay_us(1);
}
uint Write_ADS1118(uint dat,uchar mode)
{
	uint tmpa,tmpb,tmpc,tmpd;
	uchar i;
	if(mode == 1)//写命令，是连续转换还是单次转换
	{
		dat |= 0x8000;delay_us(1);
	}
	tmpa = dat;
    tmpc=dat;
    MISO_IN;
    delay_us(1);
	SCLK_L;delay_us(1);
	CS_L;delay_ms(1);
	delay_ms(1);
	for(i=0;i<16;i++)
	{
		if(tmpa & 0x8000)
		{
			MOSI_H;delay_us(1);
		}
		else
		{
			MOSI_L;delay_us(1);
		}
		tmpa <<= 1;
		delay_us(1);
		SCLK_H;
		delay_us(1);
		SCLK_L;
		delay_us(1);
		tmpb <<= 1;
        delay_us(1);
		if(READ_MISO)tmpb|= 0x01;
		;delay_us(1);
	}
	for(i=0;i<16;i++)
	{
		if(tmpc & 0x8000)
		{
			MOSI_H;delay_us(1);
		}
		else
		{
			MOSI_L;delay_us(1);
		}
		tmpc <<= 1;
		delay_us(1);
		SCLK_H;
		delay_us(1);
		SCLK_L;
		delay_us(1);
		tmpd <<= 1;
        delay_us(1);
		if(READ_MISO)tmpd|= 0x01;
	}
	CS_H;
	return tmpb;
}
/*******************************************************************************
//函数名称：ADS_SEL_Read（）
//函数功能：读取各路电压，通过两个switch选择读取不同的通道
//输    入：road:增益放大器两端的电压选择，并选择测几路电压
//          Ref: 选择参考电压，有6种选择
//输    出：dat：16位ad转换数据
//备    注：这一次读出的转换数据是上一次的转换数据，不要混淆.这里选择的是单次
            转换电压值，当然，也可以选择多次转换,通过寄存器的第8位可以设置
//日    期：2013.6.8
*******************************************************************************/
int ADS_SEL_Read(uchar CH1,uchar CH2,uchar Ref)         //测几路电压
{
    uint dat = 0;
    uint Config_Value = 0x000b;                      //默认低8位，DOUT带上拉电阻
    //配置选择通道
    if((CH1==0)&(CH2==0)) Config_Value += 0x4000;    //AINP = AIN0 and AINN = GND
    if((CH1==1)&(CH2==0)) Config_Value += 0x5000;    //AINP = AIN1 and AINN = GND
    if((CH1==2)&(CH2==0)) Config_Value += 0x6000;    //AINP = AIN2 and AINN = GND
    if((CH1==3)&(CH2==0)) Config_Value += 0x7000;    //AINP = AIN3 and AINN = GND
    if((CH1==0)&(CH2==1)) Config_Value += 0x0000;    //AINP = AIN0 and AINN = AIN1 (default)
    if((CH1==0)&(CH2==3)) Config_Value += 0x1000;    //AINP = AIN0 and AINN = AIN3
    if((CH1==1)&(CH2==3)) Config_Value += 0x2000;    //AINP = AIN1 and AINN = AIN3
    if((CH1==2)&(CH2==3)) Config_Value += 0x3000;    //AINP = AIN2 and AINN = AIN3
    //配合测量范围
    switch(Ref)
    {
		case 0:  Config_Value += 0x0000;break;    //000 : FS = ±6.144V(1)
		case 1:  Config_Value += 0x0200;break;    //001 : FS = ±4.096V(1)
		case 2:  Config_Value += 0x0400;break;    //002 : FS = ±2.048V(1)
		case 3:  Config_Value += 0x0600;break;    //003 : FS = ±1.024V(1)
		case 4:  Config_Value += 0x0800;break;    //004 : FS = ±0.512V(1)
		case 5: case 6: case 7: Config_Value += 0x0a00;break;    //005 : FS = ±0.256V(1)
		default : break;
    }
    CS_L;
    delay_us(1);
    dat = Write_ADS1118(Config_Value,1);
    delay_us(1);
    CS_H;
    delay_us(1);
    return dat;
}
char* itoa(int val, int base)
{
	static char buf[32] = {0};
	int i = 30;
	buf[31] = '\0';
	if(val==0)
	{
		buf[30] = '0';
		i--;
	}
	else
	{
		for(; val && i ; --i, val /= base)
			buf[i] = "0123456789abcdef"[val % base];
	}
	return &buf[i+1];
}
char* floatToString(float temp)
{
	static char buff[13] = {0};
	char float_char[10] = {'0','1','2','3','4','5','6','7','8','9'};
	buff[0] = 'V';
	buff[1] = 'o';
	buff[2] = 'l';
	buff[3] = ':';
	buff[8] = '.';
	buff[9] = '1';
	buff[10] = 'm';
	buff[11] = 'v';
	buff[12] = '\0';
	int i=0;
	for(i=4;i<=7;i++)
	{
		int j=(((int)temp)%(10*(8-i)))/(10*(7-i));
		buff[i] =float_char[j];
	}
	return &buff[0];
}

