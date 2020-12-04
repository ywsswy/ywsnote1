/***************************************************************************//**
 * @brief  Allows the user to adjust the backlight brightness
 * @param  none
 * @return none
 ******************************************************************************/
/*
void BacklightSetting(void)
{
    uint8_t brightness = *((unsigned char *)brightnessSetpointAddress);
    buttonsPressed = 0;
    Dogs102x6_clearScreen();
    Dogs102x6_stringDraw(0, 0, "Adjust brightness", DOGS102x6_DRAW_NORMAL);
    Dogs102x6_stringDraw(1, 0, "by turning wheel.", DOGS102x6_DRAW_NORMAL);
    Dogs102x6_imageDraw(lightbulb, 2, 35);                   //显示灯泡图案
    while (!buttonsPressed)
    {
        brightness = Wheel_getValue() / 340;                 //通过齿轮电位计采样得到需设置的背光参数
        if (brightness > 12)
            brightness = 12;                                 //背光参数最大为12
        Dogs102x6_setBacklight(brightness);                  //重置背光值
    }
    WriteFlashSettings(brightness, brightnessSetpointAddress); //将新背光值写入FLASH地址，保存设置
    buttonsPressed = 0;
    Dogs102x6_clearScreen();
}*/

