/*
 * main.c
 */
#include <stdint.h>
#include "msp430.h"
#include "HAL_Dogs102x6.h"
#include "lab1.h"
#include "YAN0317.h"
#include "YAN0317VAR.h"
#include "HAL_PMM.h"
static const char *const clockMenuText[] = {
    "== R & F meter ==",
    "1. Contrast ",
    "2. F:AUTO   ",
    "3. F:500nF  ",
    "4. F:50nF   ",
    "5. R:AUTO   ",
    "6. R:50k    ",
    "7. R:5k     ",
    "8. R:500    ",
    "9. F:choose ",
    "10.R:choose "
};
void main(void)
{
    uint8_t contrast = *((unsigned char *)contrastSetpointAddress);
    // Stop WDT
    WDTCTL = WDTPW + WDTHOLD;                     //关闭看门狗
    Ytotalitem = 10;
    Yonepageitems = 7;
    Ynowpagefirstitem=1;
	SetVCore(3);                                  //设VCore
    // Basic GPIO initialization
    Board_init();                                 //初始化GPIO(通用端口的输入输出）
    yBoard_init();
    yButtons_init(BUTTON_ALL);                    //初始化按键
    yButtons_interruptEnable(BUTTON_ALL);         //使能所有按键中断
    buttonsPressed = 0;                           //键值清零
    SFRIFG1 = 0;                                 //清中断标志
    _EINT();
    // Set up LCD
    Dogs102x6_init();                            //初始化LCD// Contrast not programed in Flash Yet
    if (contrast == 0xFF)                        //若当前FLASH中无对比度值，则将对比度值设为11（默认）
        contrast = 11;
    Dogs102x6_setContrast(contrast);             //设置初始对比度值
    Dogs102x6_clearScreen();                     //清屏
    yWheel_init();                                //初始化齿轮电位计
    Dogs102x6_clearScreen();
    buttonsPressed = 0;
    while (!(buttonsPressed & BUTTON_S2))
    {
        __delay_cycles(2000);
    	Dogs102x6_stringDraw(3, 0, "Press S2 to Menu.", DOGS102x6_DRAW_NORMAL);
    }
    while(1)
    {
		Yselection = 0;
		buttonsPressed = 0;
		Dogs102x6_clearScreen();                         //清屏
		//Yselection = yMenu_active((char **)clockMenuText, 7);   //显示菜单，并返回菜单选项
		Yselection = yMenu_active((char **)clockMenuText);   //显示菜单，并返回菜单选项
		yMenu();
    }
}
//3.618236
//3.616861
//3.617110
//3.6207
//3.6208

