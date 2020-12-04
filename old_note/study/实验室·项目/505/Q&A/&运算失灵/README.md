while (!(Ybuttonspressed & BUTTON_S1))
    {
    	yWelcome();
    }
得不到正确答案
因为定义：
#define BUTTON_S1       0x0080
uint8_t Ybuttonspressed;
Ybuttonspressed = PAIFG & BUTTON_ALL;
//SFR_16BIT(PAIFG); 
修改为:
volatile uint16_t Ybuttonspressed;
并且直接判断Ybuttonspressed & BUTTON_S1
