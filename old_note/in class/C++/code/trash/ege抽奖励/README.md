#include <ege.h>
#include <math.h>
#include <stdio.h>
#include <time.h>
#include <stdlib.h>
using namespace ege;
#define Rad 250
#define Bor 20
#define NUM 6
#define PI 3.141592653589794626
struct ITEM{
	char name[30];	//名称
	float value;	//比重
	int type;		//0 value当作数值；1 value当作百分比
};
struct ITEM item[NUM]={
	{"反转",5,1},
	{"逃过一劫",30,1},
	{"爆照",40,0},
	{"唱首歌",30,0},
	{"敲Hello World",30,0},
	{"背C语言定义",30,0},
};
double percent = 0.0;
double nowpercent = 0.0;
double total = 0.0;
char ystr[100] = "";
double buf = 0;
int cal(){
	for(int i = 0;i<NUM;i++){
		if(item[i].type == 1){
			percent += item[i].value;
		}
		else{
			total += item[i].value;
		}
	}
	if(percent > 100){
		return 0;
	}
	else{
		return 1;
	}
}
void draw(){
	double x = 0.0;
	double y = 0.0;
	double bx = Rad+Bor;
	double by = Bor;
	setlinestyle(PS_SOLID,NULL,3);
	setcolor(EGERGB(0x0, 0x0, 0x0));
	line(Rad+Bor,Rad+Bor,Rad+Bor,Bor);
	for(int i = 0;i<NUM;i++){
		if(item[i].type == 1){
			nowpercent += item[i].value;
		}
		else{
			nowpercent += ((100.0-percent)*item[i].value/total);
		}
		buf = sin(180.0*PI*nowpercent/100.0);
		x = Rad+Bor+(-sin(2*PI*nowpercent/100.0))*(-Rad);
		y = Rad+Bor+(cos(2*PI*nowpercent/100.0))*(-Rad);
		line(Rad+Bor,Rad+Bor,x,y);
		outtextxy((x+bx)/2,(y+by)/2,item[i].name);
		bx = x;
		by = y;
	}
	return;
}
void mainloop(){
	int flag = 0;
	int fps = 300+random(600);
	double i = 0;
	double x = 0.0;
	double y = 0.0; 
	double bx = Rad+Bor;
	double by = Rad+Bor;
	key_msg k = {0};
    for ( ; is_run(); delay_fps(fps) ){
		if(flag%2 == 1){
			i++;
			draw();
			x = Rad+Bor+(-sin(2*PI*i/803.0))*(-Rad*0.8);
			y = Rad+Bor+(cos(2*PI*i/803.0))*(-Rad*0.8);
			setlinestyle(PS_SOLID,NULL,5);
			setcolor(EGERGB(0xFF, 0xFF, 0xFF));
			line(Rad+Bor,Rad+Bor,bx,by);
			setcolor(EGERGB(0x0, 0x0, 0xFF));
			line(Rad+Bor,Rad+Bor,x,y);
			bx = x;
			by = y;
			if((int)(i)%(fps/30) == 0){
				fps--;
			}
		}
		if (kbmsg() && getkey().msg == key_msg_down){
			flag++;
		}
		if(fps < 30){
			flag++;
			fps = 30;
		}
    }
	return;
}
int main(){
    setinitmode(INIT_ANIMATION);
    // 图形初始化，窗口尺寸640x480
    initgraph(2*(Rad+Bor),2*(Rad+Bor));
    // 随机数初始化，如果需要使用随机数的话
    randomize();
	// 设置背景色，白色
	setbkcolor(EGERGB(0xFF, 0xFF, 0xFF)); 
	setcolor(EGERGB(0x0, 0x0, 0x0));
	setfont(25, 0, "宋体",NULL,0,700,false,false,false);
	// 程序预处理
	setlinestyle(PS_SOLID,NULL,8);
	circle(Rad+Bor,Rad+Bor,Rad);
	// 程序主循环
	if(cal()){
		draw();
		mainloop();
	}
    // 关闭绘图设备
    closegraph();
    return 0;
}
