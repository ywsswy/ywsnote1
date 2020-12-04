#include <ege.h>
#define STU_NUM 18
char name[STU_NUM][8] = {
	"彭广琳","王睿","孙仲达","安治虹",
	"贺文","张崧岳","童牧之","张烨",
	"李玉菲","张焜焱","许露","张峻",
	"王元新","乔菲菲","肖子雨","路垚",
	"刘德澳","凌宇飞"
};
void mainloop(){
	int flag = 1;
	int i = 0;
	int test = 0;
	ege::key_msg k = {0};
    for ( ; ege::is_run(); ege::delay_fps(10) ){
		if(flag%2 == 1){
			ege::cleardevice();
			i++;
			ege::outtextxy(0, 0, name[i%STU_NUM]);
		}
		if (ege::kbmsg() && ege::getkey().msg == ege::key_msg_down){
			flag++;
		}
    }
	return;
}
int main(){
    ege::setinitmode(ege::INIT_ANIMATION);
    // 图形初始化，窗口尺寸640x480
    ege::initgraph(640, 480);
    // 随机数初始化，如果需要使用随机数的话
    ege::randomize();
	//设置背景色，白色
	ege::setbkcolor(EGERGB(0xFF, 0xFF, 0xFF)); 
	ege::setcolor(EGERGB(0x0, 0x0, 0x0));
	ege::setfont(150, 0, "宋体");
    // 程序主循环
    mainloop();
    // 关闭绘图设备
    ege::closegraph();
    return 0;
}
