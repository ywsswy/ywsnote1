#include "msp430f5529.h"
#include "System_Init.h"
#include "HAL_Dogs102x6.h"
#include "ADS1118.h"
#include "Timer.h"
void main(void)
{
    System_Init();					   	// 系统初始化
    LCD_Init();							// LCD初始化
    ADS1118_Init();						// ADS初始化
    Timer_Init();						// 启动定时器
   while(1);
}

