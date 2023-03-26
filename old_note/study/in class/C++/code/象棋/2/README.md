#include<stdio.h>
#include<string.h>
#define YTEST
#ifdef YTEST
#include<windows.h>
#endif
//下标（唯一标定某个棋子），共三十二个棋子，就有三十二个下标
//命名规则：Y_<name>_<group>_<num>
#define Y_CHE_0_1 0
#define Y_CHE_0_2 1
#define Y_MA_0_1 2
#define Y_MA_0_2 3
#define Y_XIA_0_1 4
#define Y_XIA_0_2 5
#define Y_SHI_0_1 6
#define Y_SHI_0_2 7
#define Y_SHU_0_1 8
#define Y_PAO_0_1 9
#define Y_PAO_0_2 10
#define Y_BIN_0_1 11
#define Y_BIN_0_2 12
#define Y_BIN_0_3 13
#define Y_BIN_0_4 14
#define Y_BIN_0_5 15
#define Y_CHE_1_1 16
#define Y_CHE_1_2 17
#define Y_MA_1_1 18
#define Y_MA_1_2 19
#define Y_XIA_1_1 20
#define Y_XIA_1_2 21
#define Y_SHI_1_1 22
#define Y_SHI_1_2 23
#define Y_SHU_1_1 24
#define Y_PAO_1_1 25
#define Y_PAO_1_2 26
#define Y_BIN_1_1 27
#define Y_BIN_1_2 28
#define Y_BIN_1_3 29
#define Y_BIN_1_4 30
#define Y_BIN_1_5 31
//类型（不同棋子类型可能相同，例如，十个“兵”和“卒”属于同一类型YT_BIN
#define YT_CHE 1
#define YT_MA 2
#define YT_XIA 3
#define YT_SHI 4
#define YT_SHU 5
#define YT_PAO 6
#define YT_BIN 7
//棋子类
typedef struct yChess{  
	int index;//下标,唯一标识该棋子,见#define Y_PAO_0_1 等宏定义
	int type;//表示类型,见#define YT_MA 等宏定义
	char name[4];//"车"、"马"、等
	int x;//0~8
	int y;//0~9
	int alive;//0死 1活
	int group;//0甲（上）方棋子 1乙（下）方棋子
    int (*go)(struct yChess *th,int x,int y);  //下一步想走向(x,y)坐标
}yChess; 
int yGrid[10][9];//-1 不存在棋子，0~31对应不同棋子的下标
yChess yc[32];
int yDir[4][2] = {-1,-1,-1,1,1,1,1,-1};
void yInitGrid();//初始化棋盘
void yInitChess();//初始化棋子
int yGo(int x1,int y1,int x2,int y2);//走棋事件：从(x1,y1)走向(x2,y2)，x=0~8,y=0~9
int yGoChe(yChess *th,int x,int y);//“车”棋子的走棋事件
int yGoMa(yChess *th,int x,int y);//“马”棋子的走棋事件
int yGoXia(yChess *th,int x,int y);//“象”棋子的走棋事件
int yGoShi(yChess *th,int x,int y);//“士”棋子的走棋事件
int yGoShu(yChess *th,int x,int y);//“帅”棋子的走棋事件
int yGoPao(yChess *th,int x,int y);//“炮”棋子的走棋事件
int yGoBin(yChess *th,int x,int y);//“兵”棋子的走棋事件
int yJudgeGroup(int mine,int op);//判断两个棋子是敌还是友//0友 1敌
int yJudgePath(int type,int start,int end,int ex);//判断直线上两点间有多少棋子type（0横向 1纵向）
int yJudgeRiver(int index,int y);//判断这纵坐标在哪方、是否过河（0己方 1对方）
int yJudgeCamp(int index,int x,int y);//判断是否出大本营（0正常 1出了）
int main(){
	int i = 0,j = 0;
	int x1,y1,x2,y2;
	int re;
	//初始化窗口
#ifdef YTEST
	system("color f0");
	system("mode con cols=40");
	system("mode con lines=20");
#endif
	//初始化棋盘与棋子
	yInitGrid();
	yInitChess();
	while(1){
		//绘制当前棋局
#ifdef YTEST
		system("cls");
		//方法一：画棋子
		//方法二：画棋盘
		for(i = 0;i<10;i++){
			for(j = 0;j<9;j++){
				if(yGrid[i][j] == 0){
					printf("车0");
				}
				else if(yGrid[i][j] == 1){
					printf("车0");
				}
				else if(yGrid[i][j] == 2){
					printf("马0");
				}
				else if(yGrid[i][j] == 3){
					printf("马0");
				}
				else if(yGrid[i][j] == 4){
					printf("象0");
				}
				else if(yGrid[i][j] == 5){
					printf("象0");
				}
				else if(yGrid[i][j] == 6){
					printf("士0");
				}
				else if(yGrid[i][j] == 7){
					printf("士0");
				}
				else if(yGrid[i][j] == 8){
					printf("帅0");
				}
				else if(yGrid[i][j] == 9){
					printf("炮0");
				}
				else if(yGrid[i][j] == 10){
					printf("炮0");
				}
				else if(yGrid[i][j] == 11){
					printf("兵0");
				}
				else if(yGrid[i][j] == 12){
					printf("兵0");
				}
				else if(yGrid[i][j] == 13){
					printf("兵0");
				}
				else if(yGrid[i][j] == 14){
					printf("兵0");
				}
				else if(yGrid[i][j] == 15){
					printf("兵0");
				}
				else if(yGrid[i][j] == 16){
					printf("车1");
				}
				else if(yGrid[i][j] == 17){
					printf("车1");
				}
				else if(yGrid[i][j] == 18){
					printf("马1");
				}
				else if(yGrid[i][j] == 19){
					printf("马1");
				}
				else if(yGrid[i][j] == 20){
					printf("象1");
				}
				else if(yGrid[i][j] == 21){
					printf("象1");
				}
				else if(yGrid[i][j] == 22){
					printf("士1");
				}
				else if(yGrid[i][j] == 23){
					printf("士1");
				}
				else if(yGrid[i][j] == 24){
					printf("帅1");
				}
				else if(yGrid[i][j] == 25){
					printf("炮1");
				}
				else if(yGrid[i][j] == 26){
					printf("炮1");
				}
				else if(yGrid[i][j] == 27){
					printf("兵1");
				}
				else if(yGrid[i][j] == 28){
					printf("兵1");
				}
				else if(yGrid[i][j] == 29){
					printf("兵1");
				}
				else if(yGrid[i][j] == 30){
					printf("兵1");
				}
				else if(yGrid[i][j] == 31){
					printf("兵1");
				}
				else{
					printf("   ");
				}
			}
			printf("\n");
		}
#endif
		//等待走棋事件
		scanf("%d%d%d%d",&x1,&y1,&x2,&y2);
		//走棋的过程处理
		re = yGo(x1,y1,x2,y2);
	}
	return 0;
}
void yInitGrid(){
	int i = 0,j = 0;
	for(i = 0;i<10;i++){
		for(j = 0;j<9;j++){
			yGrid[i][j] = -1;
		}
	}
	return;
}
void yInitChess(){
	//依次设置棋子属性，并在棋盘数组上标定
	int i = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_CHE_0_1].go = yGoChe;
	strcpy(yc[Y_CHE_0_1].name,"车");
	yc[Y_CHE_0_1].type = YT_CHE;
	yc[Y_CHE_0_1].x = 0;
	yc[Y_CHE_0_1].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_CHE_0_2].go = yGoChe;
	strcpy(yc[Y_CHE_0_2].name,"车");
	yc[Y_CHE_0_2].type = YT_CHE;
	yc[Y_CHE_0_2].x = 8;
	yc[Y_CHE_0_2].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_MA_0_1].go = yGoMa;
	strcpy(yc[Y_MA_0_1].name,"马");
	yc[Y_MA_0_1].type = YT_MA;
	yc[Y_MA_0_1].x = 1;
	yc[Y_MA_0_1].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_MA_0_2].go = yGoMa;
	strcpy(yc[Y_MA_0_2].name,"马");
	yc[Y_MA_0_2].type = YT_MA;
	yc[Y_MA_0_2].x = 7;
	yc[Y_MA_0_2].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_XIA_0_1].go = yGoXia;
	strcpy(yc[Y_XIA_0_1].name,"象");
	yc[Y_XIA_0_1].type = YT_XIA;
	yc[Y_XIA_0_1].x = 2;
	yc[Y_XIA_0_1].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_XIA_0_2].go = yGoXia;
	strcpy(yc[Y_XIA_0_2].name,"象");
	yc[Y_XIA_0_2].type = YT_XIA;
	yc[Y_XIA_0_2].x = 6;
	yc[Y_XIA_0_2].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_SHI_0_1].go = yGoShi;
	strcpy(yc[Y_SHI_0_1].name,"士");
	yc[Y_SHI_0_1].type = YT_SHI;
	yc[Y_SHI_0_1].x = 3;
	yc[Y_SHI_0_1].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_SHI_0_2].go = yGoShi;
	strcpy(yc[Y_SHI_0_2].name,"士");
	yc[Y_SHI_0_2].type = YT_SHI;
	yc[Y_SHI_0_2].x = 5;
	yc[Y_SHI_0_2].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_SHU_0_1].go = yGoShu;
	strcpy(yc[Y_SHU_0_1].name,"帅");
	yc[Y_SHU_0_1].type = YT_SHU;
	yc[Y_SHU_0_1].x = 4;
	yc[Y_SHU_0_1].y = 0;
	//////////////////////////////////////////////////////////////
	yc[Y_PAO_0_1].go = yGoPao;
	strcpy(yc[Y_PAO_0_1].name,"炮");
	yc[Y_PAO_0_1].type = YT_PAO;
	yc[Y_PAO_0_1].x = 1;
	yc[Y_PAO_0_1].y = 2;
	//////////////////////////////////////////////////////////////
	yc[Y_PAO_0_2].go = yGoPao;
	strcpy(yc[Y_PAO_0_2].name,"炮");
	yc[Y_PAO_0_2].type = YT_PAO;
	yc[Y_PAO_0_2].x = 7;
	yc[Y_PAO_0_2].y = 2;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_0_1].go = yGoBin;
	strcpy(yc[Y_BIN_0_1].name,"兵");
	yc[Y_BIN_0_1].type = YT_BIN;
	yc[Y_BIN_0_1].x = 0;
	yc[Y_BIN_0_1].y = 3;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_0_2].go = yGoBin;
	strcpy(yc[Y_BIN_0_2].name,"兵");
	yc[Y_BIN_0_2].type = YT_BIN;
	yc[Y_BIN_0_2].x = 2;
	yc[Y_BIN_0_2].y = 3;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_0_3].go = yGoBin;
	strcpy(yc[Y_BIN_0_3].name,"兵");
	yc[Y_BIN_0_3].type = YT_BIN;
	yc[Y_BIN_0_3].x = 4;
	yc[Y_BIN_0_3].y = 3;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_0_4].go = yGoBin;
	strcpy(yc[Y_BIN_0_4].name,"兵");
	yc[Y_BIN_0_4].type = YT_BIN;
	yc[Y_BIN_0_4].x = 6;
	yc[Y_BIN_0_4].y = 3;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_0_5].go = yGoBin;
	strcpy(yc[Y_BIN_0_5].name,"兵");
	yc[Y_BIN_0_5].type = YT_BIN;
	yc[Y_BIN_0_5].x = 8;
	yc[Y_BIN_0_5].y = 3;
	//////////////////////////////////////////////////////////////
	yc[Y_CHE_1_1].go = yGoChe;
	strcpy(yc[Y_CHE_1_1].name,"车");
	yc[Y_CHE_1_1].type = YT_CHE;
	yc[Y_CHE_1_1].x = 0;
	yc[Y_CHE_1_1].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_CHE_1_2].go = yGoChe;
	strcpy(yc[Y_CHE_1_2].name,"车");
	yc[Y_CHE_1_2].type = YT_CHE;
	yc[Y_CHE_1_2].x = 8;
	yc[Y_CHE_1_2].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_MA_1_1].go = yGoMa;
	strcpy(yc[Y_MA_1_1].name,"马");
	yc[Y_MA_1_1].type = YT_MA;
	yc[Y_MA_1_1].x = 1;
	yc[Y_MA_1_1].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_MA_1_2].go = yGoMa;
	strcpy(yc[Y_MA_1_2].name,"马");
	yc[Y_MA_1_2].type = YT_MA;
	yc[Y_MA_1_2].x = 7;
	yc[Y_MA_1_2].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_XIA_1_1].go = yGoXia;
	strcpy(yc[Y_XIA_1_1].name,"象");
	yc[Y_XIA_1_1].type = YT_XIA;
	yc[Y_XIA_1_1].x = 2;
	yc[Y_XIA_1_1].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_XIA_1_2].go = yGoXia;
	strcpy(yc[Y_XIA_1_2].name,"象");
	yc[Y_XIA_1_2].type = YT_XIA;
	yc[Y_XIA_1_2].x = 6;
	yc[Y_XIA_1_2].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_SHI_1_1].go = yGoShi;
	strcpy(yc[Y_SHI_1_1].name,"士");
	yc[Y_SHI_1_1].type = YT_SHI;
	yc[Y_SHI_1_1].x = 3;
	yc[Y_SHI_1_1].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_SHI_1_2].go = yGoShi;
	strcpy(yc[Y_SHI_1_2].name,"士");
	yc[Y_SHI_1_2].type = YT_SHI;
	yc[Y_SHI_1_2].x = 5;
	yc[Y_SHI_1_2].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_SHU_1_1].go = yGoShu;
	strcpy(yc[Y_SHU_1_1].name,"帅");
	yc[Y_SHU_1_1].type = YT_SHU;
	yc[Y_SHU_1_1].x = 4;
	yc[Y_SHU_1_1].y = 9;
	//////////////////////////////////////////////////////////////
	yc[Y_PAO_1_1].go = yGoPao;
	strcpy(yc[Y_PAO_1_1].name,"炮");
	yc[Y_PAO_1_1].type = YT_PAO;
	yc[Y_PAO_1_1].x = 1;
	yc[Y_PAO_1_1].y = 7;
	//////////////////////////////////////////////////////////////
	yc[Y_PAO_1_2].go = yGoPao;
	strcpy(yc[Y_PAO_1_2].name,"炮");
	yc[Y_PAO_1_2].type = YT_PAO;
	yc[Y_PAO_1_2].x = 7;
	yc[Y_PAO_1_2].y = 7;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_1_1].go = yGoBin;
	strcpy(yc[Y_BIN_1_1].name,"兵");
	yc[Y_BIN_1_1].type = YT_BIN;
	yc[Y_BIN_1_1].x = 0;
	yc[Y_BIN_1_1].y = 6;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_1_2].go = yGoBin;
	strcpy(yc[Y_BIN_1_2].name,"兵");
	yc[Y_BIN_1_2].type = YT_BIN;
	yc[Y_BIN_1_2].x = 2;
	yc[Y_BIN_1_2].y = 6;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_1_3].go = yGoBin;
	strcpy(yc[Y_BIN_1_3].name,"兵");
	yc[Y_BIN_1_3].type = YT_BIN;
	yc[Y_BIN_1_3].x = 4;
	yc[Y_BIN_1_3].y = 6;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_1_4].go = yGoBin;
	strcpy(yc[Y_BIN_1_4].name,"兵");
	yc[Y_BIN_1_4].type = YT_BIN;
	yc[Y_BIN_1_4].x = 6;
	yc[Y_BIN_1_4].y = 6;
	//////////////////////////////////////////////////////////////
	yc[Y_BIN_1_5].go = yGoBin;
	strcpy(yc[Y_BIN_1_5].name,"兵");
	yc[Y_BIN_1_5].type = YT_BIN;
	yc[Y_BIN_1_5].x = 8;
	yc[Y_BIN_1_5].y = 6;
	//////////////////////////////////////////////////////////////
	for(i = 0;i<32;i++){
		yc[i].alive = 1;
		yc[i].index = i;
		yGrid[yc[i].y][yc[i].x] = yc[i].index;
		if(i<16){
			yc[i].group = 0;
		}
		else{
			yc[i].group = 1;
		}
	}
}
int yGoChe(yChess *th,int x,int y){
	//必须横走或直走
	if(th->x != x && th->y != y){
		return -1;
	}
	//路径上不能有障碍物
	if(th->x == x){
		if(yJudgePath(1,th->y,y,th->x) > 0){
			return -1;
		}
	}
	else{
		if(yJudgePath(0,th->x,x,th->y) > 0){
			return -1;
		}
	}
	//目的地有棋子
	if(yGrid[y][x] != -1){
		//不能吃掉己方的棋子
		if(yJudgeGroup(th->index,yGrid[y][x]) == 0){
			return -1;
		}
		//可以吃掉对方的棋子
		else{
			yGrid[th->y][th->x] = -1;
			th->x = x;
			th->y = y;
			yc[yGrid[th->y][th->x]].alive = 0;
			yGrid[th->y][th->x] = th->index;
		}
	}
	//目的地没有棋子，直接移动
	else{
		yGrid[th->y][th->x] = -1;
		th->x = x;
		th->y = y;
		yGrid[th->y][th->x] = th->index;
	}
	return 0;
}
int yGoMa(yChess *th,int x,int y){
	//必须走日字
	if(	
		(
			(
				(th->x-x == 2)||(th->x-x == -2)
			)
			&&
			(
				(th->y-y == 1)||(th->y-y == -1)
			)
		)
		||
		(
			(
				(th->x-x == 1)||(th->x-x == -1)
			)
			&&
			(
				(th->y-y == 2)||(th->y-y == -2)
			)
		)		
		){
		//不能鳖马脚
		if(x-th->x == 2 && yGrid[th->y][th->x+1] != -1){
			return -1;
		}
		if(x-th->x == -2 && yGrid[th->y][th->x-1] != -1){
			return -1;
		}
		if(y-th->y == 2 && yGrid[th->y+1][th->x] != -1){
			return -1;
		}
		if(y-th->y == -2 && yGrid[th->y-1][th->x] != -1){
			return -1;
		}
		//目的地有棋子
		if(yGrid[y][x] != -1){
			//不能吃掉己方的棋子
			if(yJudgeGroup(th->index,yGrid[y][x]) == 0){
				return -1;
			}
			//可以吃掉对方的棋子
			else{
				yGrid[th->y][th->x] = -1;
				th->x = x;
				th->y = y;
				yc[yGrid[th->y][th->x]].alive = 0;
				yGrid[th->y][th->x] = th->index;
			}
		}
		//目的地没有棋子，直接移动
		else{
			yGrid[th->y][th->x] = -1;
			th->x = x;
			th->y = y;
			yGrid[th->y][th->x] = th->index;
		}
	}
	else{
		return -1;
	}
}
int yGoXia(yChess *th,int x,int y){
	int dir = 0;
	//方向：逆时针，从左上角开始0-3
	//不能过河
	if(yJudgeRiver(th->index,y)){
		return -1;
	}
	//必须走田字
	if(th->x-x != 2 && th->x-x != -2){
		return -1;
	}
	if(th->y-y != 2 && th->y-y != -2){
		return -1;
	}
	//确定四个方向之一
	if(x-th->x<0 && y-th->y>0){
		dir = 1;
	}
	else if(x-th->x>0 && y-th->y>0){
		dir = 2;
	}
	else if(x-th->x>0 && y-th->y<0){
		dir = 3;
	}
	//不能鳖象眼
	if(yGrid[th->y+yDir[dir][1]][th->x+yDir[dir][0]] != -1){
		return -1;
	}
	//目的地有棋子
	if(yGrid[y][x] != -1){
		//不能吃掉己方的棋子
		if(yJudgeGroup(th->index,yGrid[y][x]) == 0){
			return -1;
		}
		//可以吃掉对方的棋子
		else{
			yGrid[th->y][th->x] = -1;
			th->x = x;
			th->y = y;
			yc[yGrid[th->y][th->x]].alive = 0;
			yGrid[th->y][th->x] = th->index;
		}
	}
	//目的地没有棋子，直接移动
	else{
		yGrid[th->y][th->x] = -1;
		th->x = x;
		th->y = y;
		yGrid[th->y][th->x] = th->index;
	}
	return 0;
}
int yGoShi(yChess *th,int x,int y){
	//必须走斜线
	if(th->x-x != 1 && th->x-x != -1){
		return -1;
	}
	if(th->y-y != 1 && th->y-y != -1){
		return -1;
	}
	//不能出大本营
	if(yJudgeCamp(th->index,x,y)){
		return -1;
	}
	//目的地有棋子
	if(yGrid[y][x] != -1){
		//不能吃掉己方的棋子
		if(yJudgeGroup(th->index,yGrid[y][x]) == 0){
			return -1;
		}
		//可以吃掉对方的棋子
		else{
			yGrid[th->y][th->x] = -1;
			th->x = x;
			th->y = y;
			yc[yGrid[th->y][th->x]].alive = 0;
			yGrid[th->y][th->x] = th->index;
		}
	}
	//目的地没有棋子，直接移动
	else{
		yGrid[th->y][th->x] = -1;
		th->x = x;
		th->y = y;
		yGrid[th->y][th->x] = th->index;
	}
	return 0;
}
int yGoShu(yChess *th,int x,int y){
	//必须横走或直走
	if(th->x != x && th->y != y){
		return -1;
	}
	//不能出大本营
	if(yJudgeCamp(th->index,x,y)){
		return -1;
	}
	//每次只能走一格
	if(th->x-x == 0 && (th->y-y>1 || th->y-y<-1)){
		return -1;
	}
	if(th->y-y == 0 && (th->x-x>1 || th->x-x<-1)){
		return -1;
	}
	//目的地有棋子
	if(yGrid[y][x] != -1){
		//不能吃掉己方的棋子
		if(yJudgeGroup(th->index,yGrid[y][x]) == 0){
			return -1;
		}
		//可以吃掉对方的棋子
		else{
			yGrid[th->y][th->x] = -1;
			th->x = x;
			th->y = y;
			yc[yGrid[th->y][th->x]].alive = 0;
			yGrid[th->y][th->x] = th->index;
		}
	}
	//目的地没有棋子，直接移动
	else{
		yGrid[th->y][th->x] = -1;
		th->x = x;
		th->y = y;
		yGrid[th->y][th->x] = th->index;
	}
	return 0;
}
int yGoPao(yChess *th,int x,int y){
	int num;
	//必须横走或直走
	if(th->x != x && th->y != y){
		return -1;
	}
	//计算路径有障碍物
	if(th->x == x){
		num = yJudgePath(1,th->y,y,th->x);
	}
	else{
		num = yJudgePath(0,th->x,x,th->y);
	}
	//最多隔一个棋子
	if(num > 1){
		return -1;
	}
	//目的地有棋子
	if(yGrid[y][x] != -1){
		//要吃子必须隔一个
		if(num == 1){
			//不能吃掉己方的棋子
			if(yJudgeGroup(th->index,yGrid[y][x]) == 0){
				return -1;
			}
			//可以吃掉对方的棋子
			else{
				yGrid[th->y][th->x] = -1;
				th->x = x;
				th->y = y;
				yc[yGrid[th->y][th->x]].alive = 0;
				yGrid[th->y][th->x] = th->index;
			}
		}
		else{
			return -1;
		}
	}
	//目的地没有棋子，直接移动
	else{
		yGrid[th->y][th->x] = -1;
		th->x = x;
		th->y = y;
		yGrid[th->y][th->x] = th->index;
	}
	return 0;
}
int yGoBin(yChess *th,int x,int y){
	//确定方向
	int dir = 0;//0往下走，1往上走
	if(th->index>15){
		dir = 1;
	}
	//不能回头走
	if(dir == 1 && x-th->x>0){
		return -1;
	}
	if(dir == 0 && th->x-x>0){
		return -1;
	}
	//每次只能走一格
	if(th->x-x == 0 && (th->y-y>1 || th->y-y<-1)){
		return -1;
	}
	if(th->y-y == 0 && (th->x-x>1 || th->x-x<-1)){
		return -1;
	}
	//没过河不能横着走
	if(!yJudgeRiver(th->index,th->y) && th->x != x){
		return 0;
	}
	//目的地有棋子
	if(yGrid[y][x] != -1){
		//不能吃掉己方的棋子
		if(yJudgeGroup(th->index,yGrid[y][x]) == 0){
			return -1;
		}
		//可以吃掉对方的棋子
		else{
			yGrid[th->y][th->x] = -1;
			th->x = x;
			th->y = y;
			yc[yGrid[th->y][th->x]].alive = 0;
			yGrid[th->y][th->x] = th->index;
		}
	}
	//目的地没有棋子，直接移动
	else{
		yGrid[th->y][th->x] = -1;
		th->x = x;
		th->y = y;
		yGrid[th->y][th->x] = th->index;
	}
	return 0;
}
int yGo(int x1,int y1,int x2,int y2){
	//判断走棋是否符合不越界、不原地停留两个基本条件，不规范则返回-2
	if(x1<0 || x2<0 || y1<0 || y2<0 || x1>8 || x2>8 || y1>9 || y2>9 || (x1==x2 && y1==y2)){
		return -2;
	}
	//判断(x1,y1)处是否有棋子，没有棋子则返回-1
	if(yGrid[y1][x1] == -1){
		return -1;
	}
	//有棋子，则执行该棋子的走棋函数（错误走法，返回-1）
	return yc[yGrid[y1][x1]].go(&yc[yGrid[y1][x1]],x2,y2);
}
int yJudgeGroup(int mine,int op){
	if(mine<16 && op<16 || mine>15 && op>15){
		return 0;
	}
	else{
		return 1;
	}
}
int yJudgePath(int type,int start,int end,int ex){
	int i;
	int num = 0;
	if(start>end){
		i = end;
		end = start;
		start = i;
	}
	if(type == 0){
		for(i = start+1;i<end;i++){
			if(yGrid[ex][i] != -1){
				num++;
			}
		}
	}
	else{
		for(i = start+1;i<end;i++){
			if(yGrid[i][ex] != -1){
				num++;
			}
		}
	}
	return num;
}
int yJudgeRiver(int index,int y){
	if(index<16 && y>4 || index>15 && y<5){
		return 1;
	}
	return 0;
}
int yJudgeCamp(int index,int x,int y){
	if(index < 16){
		if(x<3 || x>5 || y>2){
			return 1;
		}
	}
	else{
		if(x<3 || x>5 || y<7){
			return 1;
		}
	}
	return 0;
}

