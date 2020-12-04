#include "Yboard.h"
#include "Yglobalvar.h"
#include "Ydogs102x6.h"
#include "Ymenu.h"
#include "Yuser.h"
uint8_t yMenu_active(){
    uint8_t i;
    uint8_t position = 0;
    uint8_t lastPosition = 255;
    yWheel_enable();
    yDogs102x6_clearScreen();
    Ybuttonspressed = 0;
    yDogs102x6_stringDraw(0, 0, Ymenutext[0], NORMAL_STYLE);
    while (!(Ybuttonspressed & BUTTON_S1)){
        position = yWheel_getPosition(Ytotalitems);
        if (position != lastPosition){
        	if(position+1 > Ynowfirstitem+Yonepageitems-1){
        		Ynowfirstitem = position+1-Yonepageitems+1;
        	}
        	else if(position+1 < Ynowfirstitem){
        		Ynowfirstitem = position+1;
        	}
        	for (i = 1; i <= Yonepageitems && i <= Ytotalitems; i++){
    			if (i-1+Ynowfirstitem == position+1){
					yDogs102x6_stringDraw(i, 0, Ymenutext[i-1+Ynowfirstitem], ROW_STYLE+INVERT_STYLE);
				}
				else {
					yDogs102x6_stringDraw(i, 0, Ymenutext[i-1+Ynowfirstitem], ROW_STYLE+NORMAL_STYLE);
				}
			}
            lastPosition = position;
        }
    }
    Ybuttonspressed = 0;
    yDogs102x6_clearScreen();
    yWheel_disable();
    return position+1;
}
//菜单界面初始化，设置选项的个数
void yMenu_init(){
    Yonepageitems = YROW-1;
    Ynowfirstitem=1;
    Ys2model = 0;
}

