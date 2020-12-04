#include "MSP430F5529.H"
#include "string.h"
#include "Yuser.h"
#include "Ymenu.h"			//【通过滚轮配合菜单选项】
#include "Yboard.h"		//【白板子各模块驱动】
#include "Ydogs102x6.h"	//【白板子液晶显示屏】内含画折线图、显示字符串、画圆、画线、画图片等函数
#include "Yglobalvar.h"	//【全局变量】如按键标志、滚轮ADC采样值、当前菜单页数等
#include "Ylib.h"			//【基本运算库】内含滤波函数、
											//整数转字符串函数、
#define YKK2 313*20
#define YKK 313*40
#define YKK4 313*20-5
#define YKK3 313*40-10
#define YH P6OUT |= BIT4;__delay_cycles(YKK)
#define YL P6OUT &= ~BIT4;__delay_cycles(YKK)
//YDNUM：设置所画折线图的X轴宽度
#define YDNUM 20
//YANUM：设置外设ADC采样次数
#define YANUM 8
//Ymenutext设置菜单每个选项的标题（每行不超过17个字符，行数可以无限多，自行添加）
char *const Ymenutext[] = {
    "==   M E N U   ==",
    "1. set contrast",
    "2. simple draw",
    "3. show number",
    "4. input num",
    "5. show curve",
	"6. ADC",
	"7. Send",
	"8. Read",
};
//全局变量，保存齿轮输入的数字
volatile float gnum1 = 0;
//外设ADC每次采样结果
volatile unsigned int yadc1[YANUM];// Needs to be global in this
										// example. Otherwise, the
										// compiler removes it because it
										// is not used for anything.
volatile uint8_t yindex1 = 0;
volatile uint16_t yindex2 = 0;
volatile uint16_t yindexbuf = 0;
volatile uint8_t ydownflag = 0;
void sendByte(uint8_t);
uint8_t readByte();
uint8_t dt = 0;								//求最大值最小值函数、
uint8_t dtem = 0xaa;
uint8_t dtem2 = 0;
//S505实验室log（matlab二值化+字模生成）（不推荐费时间在这里，如有需要，详细生成可参考http://download.csdn.net/detail/yws_swy/9581130）
static const uint8_t s505[] =
{102,8,//前两个分别代表图片像素列数和行数（每8个像素为1行）
255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,
255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,195,219,219,219,219,251,251,251,219,219,251,199,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,
255,255,255,255,255,255,255,255,255,255,255,255,255,192,223,223,223,223,255,255,191,191,191,128,255,254,254,254,254,254,254,252,254,240,247,247,255,251,251,251,255,253,253,253,254,254,253,253,253,253,253,253,249,253,254,253,251,247,247,239,239,239,239,239,175,207,207,223,175,239,239,239,227,252,255,254,254,254,254,255,255,255,252,239,224,255,255,223,255,255,192,255,255,255,255,255,255,255,255,255,255,255,
255,255,255,255,255,255,255,255,255,255,255,255,254,0,254,254,254,254,254,254,254,255,254,3,255,255,223,223,223,223,223,95,207,143,247,247,247,239,239,239,223,223,223,191,191,175,111,127,255,255,255,255,207,207,127,191,255,223,239,247,247,247,247,247,255,243,241,241,241,240,236,254,199,63,255,255,255,255,255,255,255,63,127,251,3,243,255,255,255,255,3,255,255,255,255,255,255,255,255,254,254,255,
255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,238,204,200,201,136,136,8,136,80,80,0,0,0,206,255,255,255,255,255,255,255,255,255,255,255,255,255,255,252,252,248,240,224,224,128,128,0,0,0,0,255,
255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,255,127,31,135,131,99,97,48,40,24,30,30,6,5,191,255,255,251,248,252,254,254,252,240,224,192,192,0,0,0,0,0,0,0,0,0,0,0,0,0,255,
255,255,255,231,195,219,217,204,206,255,193,193,219,219,216,220,255,224,192,223,223,192,224,255,193,193,219,219,216,220,255,158,158,186,168,164,54,32,160,190,190,158,158,255,161,161,129,128,244,227,202,26,27,202,226,247,255,199,199,214,212,80,18,150,212,212,214,199,199,127,47,1,0,0,128,192,224,224,192,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,1,3,15,15,3,0,255,
255,255,255,63,63,191,191,63,127,255,127,63,191,191,63,127,255,127,63,191,191,63,127,255,127,63,191,191,63,127,255,239,239,207,223,159,63,127,191,191,159,207,239,255,191,175,47,15,15,15,143,47,15,15,47,239,255,239,175,175,175,175,15,15,175,175,175,175,239,255,255,255,127,63,31,15,15,15,15,15,7,7,7,7,15,15,15,15,15,31,31,127,255,255,255,255,255,255,255,255,255,255,};
void yWelcome();			//初始欢迎界面（画图片例子）
void yContrastSetting();	//设置对比度（获取滚轮采样值例子）
void ySimpleDraw();		//（画圆、画直线的简单的显示例子）
void yShowNum();			//int型整数、与float型浮点数的显示方法（int float类型转字符串例子）
void yInputNum();			//（以滚轮代替键盘输入数字例子）
void yShowCurve();			//（画折线图例子）
void yADC();
void ySend();
void yRead();
//启动用户主程序
void yUserStart(){
	uint8_t selection;							//当前选项
    Ytotalitems =8;							//设置菜单选项个数
	yWelcome();									//初始欢迎界面（画图片例子）
    while(1){
        Ybuttonspressed = 0;					//清除按键标志（切记每次读取完按键标志，都要清除此标志）
		selection = yMenu_active();				//通过滚轮获取选项
		switch (selection){						//通过选项选择执行相应程序
			case 1: yContrastSetting(); break;	//设置对比度（获取滚轮采样值例子）
			case 2: ySimpleDraw(); break;		//（画圆、画直线的简单的显示例子）
			case 3: yShowNum(); break;			//int型整数、与float型浮点数的显示方法
			case 4: yInputNum(); break;		//（以滚轮代替键盘输入数字例子）
			case 5: yShowCurve();break;		//（画折线图例子）
			//可以无限往下添加case6,case7...
			case 6: yADC();break;
			case 7: ySend();break;
			case 8: yRead();break;
			default: break;
		}
    }
}
uint8_t readByte(){
	uint8_t i;
	uint8_t c = 0;
	__delay_cycles(YKK3);
	__delay_cycles(YKK4);
	for(i = 0;i<8;i++){
		if(P2IN & BIT1){
			c |= (1<<i);
		}
		else{
			c |= (0<<i);
		}
		__delay_cycles(YKK3);
	}
	P2OUT ^= BIT3;
	return c;
}
void yRead(){
	char s[18] = "";
	Ys2model = 1;
	P2SEL &=~BIT3;
	P2DIR |= BIT3;
	P2OUT |= BIT3;
	P2SEL &=~BIT1;
	P2REN |= BIT1;
	P2OUT |= BIT1;
	P2DIR &=~BIT1;
	P2IES |= BIT1;
	P2IFG &=~BIT1;
	P2IE |= BIT1;
/*
	P2SEL &=~BIT0;
	P2REN |= BIT0;
	P2OUT &=~BIT0;
	P2DIR &=~BIT0;
	P2IES &=~BIT0;
	P2IFG &=~BIT0;
	P2IE |= BIT0;
*/
	while(1){
		while(ydownflag == 0){
			yItoa(dtem2/16,s);
			yDogs102x6_stringDraw(0,8*5,s,ROW_STYLE+INVERT_STYLE);
			yItoa(dtem2%16,s);
			yDogs102x6_stringDraw(1,8*5,s,ROW_STYLE+INVERT_STYLE);
		}
	}
	Ys2model = 0;
}
void ySend(){
	char s[18] = "";
	P6DIR |= BIT4;
	while(1){
		sendByte(dtem);
		YH;
		for(dt = 0;dt<80;dt++){
			__delay_cycles(YKK);
		}
		yItoa(dtem/16,s);
		yDogs102x6_stringDraw(0,8*5,s,ROW_STYLE+INVERT_STYLE);
		yItoa(dtem%16,s);
		yDogs102x6_stringDraw(1,8*5,s,ROW_STYLE+INVERT_STYLE);
	}
}
void sendByte(uint8_t c){
	uint8_t i;
	YL;
	for(i = 0;i<8;i++){
		if(c&0x01){
			YL;
		}
		else{
			YH;
		}
		c>>=1;
	}
}
void yADC(){
	SFRIFG1 = 0;
	P8DIR |= BIT1;
	TA1CCTL0 = CCIE;//CCR0中断使能
	TA1CCR0 = 32767;
	TA1CTL = TASSEL_1 + MC_1 + TACLR;//ACLK,增计数模式，清除TAR计数器,//1hz
	while(1){
		Yadcmodel = 1;										//内部ADC滚轮使用
		ADC_PORT_DIR &= ~(ADC_INPUT_A7);					//P6.5 as input
		ADC_PORT_SEL |= ADC_INPUT_A7;                       //P6.5 ADC option select (A5)
		WHEEL_PORT_DIR |= WHEEL_ENABLE;						//P8.0 as output
		WHEEL_PORT_OUT |= WHEEL_ENABLE;                     //enable wheel
		ADC12CTL0 = ADC12SHT01 + ADC12ON;                  // Sampling time, ADC12 on
		ADC12CTL1 = ADC12SHP;             // Use sampling timer
		ADC12MCTL0 = ADC12INCH_7;                          // Use A5 (wheel) as input
		ADC12CTL0 |= ADC12ENC;                             // Enable conversions
		ADC12IE = BIT0;                                    // Enable interrupt
		ADC12CTL0 |= ADC12SC;                              // Start sampling/conversion
	}
}
//初始欢迎界面（画图片例子）
void yWelcome(){
	yDogs102x6_imageDraw(s505, 0, 0);			//画图（以LCD（0，0）坐标处为左上角把s505字模绘制出来）
	yDogs102x6_stringDraw(0, 0,"Press S2 to Menu.",NORMAL_STYLE);	//显示字符串
    while (!(Ybuttonspressed & BUTTON_S2));				//直到按下S2键才返回菜单
}
//设置对比度（获取滚轮采样值例子）
void yContrastSetting(){
    uint8_t contrast = 0;
    yWheel_enable();
    yDogs102x6_imageDraw(s505,0,0);
    yDogs102x6_stringDraw(0,0, "Turning wheel.", ROW_STYLE+NORMAL_STYLE);
    while (!(Ybuttonspressed & BUTTON_S2)){
        contrast = yWheel_getPosition(25);	//通过齿轮电位计采样（0~24之间取值）
        yDogs102x6_setContrast(contrast);	//设置对比度
    }
    yWheel_disable();
}
//画圆、画直线的简单的显示
void ySimpleDraw(){
	yDogs102x6_circleDraw(50,30,10,0);								//画圆
	yDogs102x6_lineDraw(40,30,60,30);								//画直线
	yDogs102x6_lineDraw(50,20,50,40);
	yDogs102x6_stringDraw(0, 0, "Press S2 to Menu.", ROW_STYLE+NORMAL_STYLE);
	while (!(Ybuttonspressed & BUTTON_S2));
}
//int型整数、与float型浮点数的显示方法
void yShowNum(){
	int a = -345;//int型整数范围为（-32768~32767)
	float b = 1.56;
	char s[18] = "";
	yItoa(a,s);//int类型
	yDogs102x6_stringDraw(1, 0, s, INVERT_STYLE);
	yFtoa(b,s);//float类型
	yDogs102x6_stringDraw(2, 0, s, INVERT_STYLE);
	yDogs102x6_stringDraw(0, 0, "Press S2 to Menu.", ROW_STYLE+NORMAL_STYLE);
    while (!(Ybuttonspressed & BUTTON_S2));
}
//（以滚轮代替键盘输入数字例子）
//此处通过滚轮设置了全局变量gnum1的值
void yInputNum(){
	char ystrbuf[4]="";//临时调试输出
	char ystr[20] = "";//保存每一位输入的字符
	uint8_t yloc = 0;	//当前输入到ystr的第几位
	uint8_t ydot = 0;	//小数点标志，（0=没输入小数点；1=已输入小数点）
	uint8_t yi;
	uint8_t yflag = 0;	//输入结束标志（0=继续输入；1=完成输入）
	uint8_t ybutton = 0;//S1键使能标志，（0=未按S2，不使能S1；1=已按S2，使能S1）
	uint8_t yvalue;		//滚轮的转换结果
	gnum1 = 0.0;
	yWheel_enable();		//滚轮ADC采样使能
    while (!(Ybuttonspressed & BUTTON_S1) || yflag != 1){
		yvalue = yWheel_getPosition(12-ydot);
		if(yvalue < 10){
			ystrbuf[0]='0'+yvalue;
			ystrbuf[1]=0;
			yDogs102x6_stringDraw(1, 0,ystrbuf , ROW_STYLE+NORMAL_STYLE);
		}
		else if(yvalue == 12-ydot-1){
			yDogs102x6_stringDraw(1,0,"END",ROW_STYLE+NORMAL_STYLE);
		}
		else{
			yDogs102x6_stringDraw(1, 0,".", ROW_STYLE+NORMAL_STYLE);
		}
    	if(Ybuttonspressed & BUTTON_S2){
    		ybutton = 1;
    	}
    	if((Ybuttonspressed & BUTTON_S1)&&ybutton){
    		ybutton = 0;
    		if(yvalue < 10){
    			ystr[yloc]='0'+yvalue;
    		}
    		else if(yvalue == 12-ydot-1){			//选择了END，输入结束
    			yflag = 1;
    			ystr[yloc]=' ';
    		}
    		else{
    			ystr[yloc]='.';
    		}
    		yloc++;
    	}
    	yDogs102x6_stringDraw(2,0,ystr,INVERT_STYLE);
    }
    yWheel_disable();	//滚轮ADC采样禁止
    //开始把ystr数组中的字符转换为gnum1的值
    ystr[yloc-1] = 0;
    yloc = strlen(ystr);
    for(yi = 0;ystr[yi] != 0;yi++){
    	if(ystr[yi] != '.'){
    	    gnum1 += ystr[yi]-'0';
    	    gnum1 = gnum1 * 10;
    	}
    	else{
    		yloc = yi;
    	}
    }
    gnum1 = gnum1 / 10;
    for(yloc = strlen(ystr)-yloc-1;yloc>0;yloc--){
    	gnum1 = gnum1 /10;
    }
    yFtoa(gnum1,ystr);
	yDogs102x6_stringDraw(0, 0, "Press S2 to Menu.", ROW_STYLE+NORMAL_STYLE);
    while (!(Ybuttonspressed & BUTTON_S2));
}
//（画折线图例子）
void yShowCurve(){
	uint8_t yflag = 0;	//输入结束标志（0=继续输入；1=完成输入）
	float ydatay[YDNUM] = {0};
	float ydatax[YDNUM] = {0};
	float ybuf = 0;
	int ytime = 0;
	int yrand = 0;//
	uint8_t i;
	yDogs102x6_stringDraw(0, 0, "Press S1 to start.", ROW_STYLE+NORMAL_STYLE);
    while (yflag == 0){
    	if(ytime < YDNUM){
			ydatax[ytime]=ytime;
			/////////////////////////////////////////////////
			//在实际中，是每隔一段时间测量一个数据
			//在这里是每按一次S1键，模拟产生一个新的随机数据
			Ybuttonspressed = 0;
			while (!(Ybuttonspressed & BUTTON_S1)){
				yrand++;			//模拟产生随机数
				if(Ybuttonspressed & BUTTON_S2){
					yflag = 1;
				}
			}
			ybuf = ydatay[(ytime+YDNUM-1)%YDNUM]+(float)(yrand%50-5)/10;
			if(ybuf>255){
				ybuf = 255;
			}
			else if(ybuf<0){
				ybuf = 0;
			}
			ydatay[ytime%YDNUM] = ybuf;
			//////////////////////////////////////////////////
	    	yDogs102x6_clearScreen();
			yDogs102x6_curveDraw(ydatax,ydatay,0,ytime,1,0,0,102,64,5);
			ytime++;
    	}
    	else{
    		//////////////////////////////////////////////////
			Ybuttonspressed = 0;
			while (!(Ybuttonspressed & BUTTON_S1)){
				yrand++;
				if(Ybuttonspressed & BUTTON_S2){
					yflag = 1;
				}
			}
			ybuf = ydatay[(ytime+YDNUM-1)%YDNUM]+(float)(yrand%50-5)/10;
			if(ybuf>255){
				ybuf = 255;
			}
			else if(ybuf<0){
				ybuf = 0;
			}
	    	for(i = 0;i<YDNUM-1;i++){
	    		ydatay[i] = ydatay[i+1];
	    	}
			ydatay[YDNUM-1] = ybuf;
			///////////////////////////////////////////////////
	    	yDogs102x6_clearScreen();
			yDogs102x6_curveDraw(ydatax,ydatay,0,YDNUM-1,1,0,0,102,64,5);
    	}
	}
	yDogs102x6_stringDraw(0, 0, "Press S2 to Menu.", ROW_STYLE+NORMAL_STYLE);
    while (!(Ybuttonspressed & BUTTON_S2));
}
#pragma vector=PORT1_VECTOR
__interrupt void Port1_ISR(void){
    Ybuttonspressed = PAIFG & BUTTON_ALL;
    dtem--;
    switch (__even_in_range(P1IV, P1IV_P1IFG7)){
    default:break;
    }
}
#pragma vector=PORT2_VECTOR
__interrupt void Port2_ISR(void){
    Ybuttonspressed = PAIFG & BUTTON_ALL;
    if(Ys2model == 0){
    	dtem++;
    }
    else{
    	P2OUT ^= BIT3;
    	P2IFG &=~BIT1;
    	P2IE &=~BIT1;
    	ydownflag = 1;
		dtem2 = readByte();
		ydownflag = 0;
    	P2IFG &=~BIT1;
		P2IE |= BIT1;
		ydownflag = 0;
    }
    switch (__even_in_range(P2IV, P2IV_P2IFG7)){
    default:break;
    }
}
#pragma vector = ADC12_VECTOR
__interrupt void ADC12_ISR(void){
    switch (__even_in_range(ADC12IV, ADC12IV_ADC12IFG15)){
        case  ADC12IV_ADC12IFG0:
        	if(Yadcmodel == 1){
				yadc1[yindex1] = ADC12MEM0;             // Move results
				yindex1++;                                // Increment results index, modulo; Set Breakpoint1 here
				yindex2++;
				if (yindex1 == YANUM){
					yindex1 = 0;
				}
				ADC12IE = 0x00;
				ADC_PORT_DIR |= ADC_INPUT_A7;					//P6.5 as input
				ADC_PORT_SEL &= ~ADC_INPUT_A7;                       //P6.5 ADC option select (A5)
				ADC12CTL0 &= ~ADC12ENC;
				ADC12CTL0 &= ~ADC12ON;
        	}
        	break;
        case ADC12IV_ADC12IFG1:
        	if(Yadcmodel == 0){
				Ypositiondata = ADC12MEM1;
        	}
            break;
        default:break;
    }
}
#pragma vector=TIMER1_A0_VECTOR
__interrupt void TIMERR1_A0_ISR(void)
{
	P8OUT ^= BIT1;
	yindexbuf = yindex2;
	yindex2 = 0;
}

