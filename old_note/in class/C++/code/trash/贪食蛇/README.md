#include<stdio.h>
#include<stdlib.h>
#include<windows.h>
#include<conio.h>
#include<time.h>
#define N 15
struct point{
	int x;
	int y;
	int dir;//1上 -1下 2左 -2右 相反方向为负，方便判断
	struct point *next;//下一个身体节点
};
//用二维数组显示画面
void show(int dis[][N]);
//更新显示画面（把链表存储的蛇身体和食物更新到二维数组里，并且更新食物位置）
void update(int dis[][N],struct point *temp,int *foodx,int *foody);
//初始化蛇头
struct point * init(struct point *head);
//递归实现每一个身体结点带动下一个身体结点
void move(struct point * temp,int dir,int x,int y,int flag);
//根据键盘输入进行移动
void go(struct point *head,int foodx,int foody,int ch);
int main(){
	int ch = 77;//72上 75左 77右 80下 设置初始方向为向右
	int dis[N][N] = {0};//0空地 1蛇身体 2食物
	struct point * head = NULL;
	int foodx = 0;//食物的横坐标
	int foody = 0;//食物的纵坐标
	int time1 = 0;//计时器，累加到某个值则蛇移动一次
	int flag = 0;//0没吃到食物 1吃到了食物
	srand(time(NULL));
	head = init(head);
	foodx = rand()%N;
	foody = rand()%N;
	update(dis,head,&foodx,&foody);
	while(1){	
		if(kbhit()){//判断是否有按键信息未读取
            ch = getch();
        }
		Sleep(3);//3可改，控制灵敏度
		time1++;
		if(time1 % 40 == 0){//40可改，控制移动速度
			//根据键盘输入进行移动
			go(head,foodx,foody,ch);
			//更新显示画面（把链表存储的蛇身体和食物更新到二维数组里，并且更新食物位置）
			update(dis,head,&foodx,&foody);
		}
	}
	return 0;
}
struct point * init(struct point *head){
	head = (struct point *)malloc(sizeof(struct point));
	head->next = NULL;
	head->x = rand()%N;
	head->y = rand()%N;
	head->dir = -2;
	return head;
}
void update(int dis[][N],struct point *temp,int *foodx,int *foody){
	int i = 0;
	int j = 0;
	for(i = 0;i<N;i++){
		for(j = 0;j<N;j++){
			dis[i][j] = 0;
		}
	}
	//蛇身体
	while(temp != NULL){
		dis[temp->y][temp->x] = 1;
		temp = temp->next;
	}
	//食物不能在出现在身体上
	while(dis[*foody][*foodx] != 0){
		*foody = rand()%N;
		*foodx = rand()%N;
	}
	dis[*foody][*foodx] = 2;
	//用二维数组显示画面
	show(dis);
	return;
}
void go(struct point *head,int foodx,int foody,int ch){
	int nowx = head->x;//蛇头要移动到的横坐标
	int nowy = head->y;//蛇头要移动到的纵坐标
	int nowdir = head->dir;//由当前键盘确定的蛇头要移动的方向
	int flag = 0;//0没吃到食物 1吃到了食物
	char color[] = "color 0f";//设置显示颜色
	//判断最终前进方向 72上 75左 77右 80下
	if(ch == 72){
		ch = 1;
	}
	else if(ch == 75){
		ch = 2;
	}
	else if(ch == 77){
		ch = -2;
	}
	else if(ch == 80){
		ch = -1;
	}
	else{
		ch = nowdir;
	}
	//如果按键方向不是蛇前进的反方向，则此次按键有效，不然禁止折返跑
	if(ch != -nowdir){
		nowdir = ch;
	}
	//计算蛇头新坐标（考虑边界溢出）
	if(nowdir == -2){
		nowx = (nowx+1)%N;
	}
	else if(nowdir == 2){
		nowx = (nowx-1+N)%N;
	}
	else if(nowdir == 1){
		nowy = (nowy-1+N)%N;
	}
	else{
		nowy = (nowy+1)%N;
	}
	//判断是否吃到食物，吃到食物则会在尾巴处新建节点
	if(nowx == foodx && nowy == foody){
		flag = 1;
		do{
			if(rand()%2 == 0){
				color[6] = '0'+rand()%10;
			}
			else{
				color[6] = 'a'+rand()%6;
			}
			if(rand()%2 == 0){
				color[7] = '0'+rand()%10;
			}
			else{
				color[7] = 'a'+rand()%6;
			}
		}while(color[6] == color[7]);
		system(color);
	}
	//开始移动身体
	move(head,nowdir,nowx,nowy,flag);
	return;
}
void move(struct point * temp,int dir,int x,int y,int flag){
	struct point *newp = NULL;
	//下一个身体结点的状态（位置和方向）变成当前节点的状态
	if(temp->next!=NULL){
		move(temp->next,temp->dir,temp->x,temp->y,flag);
	}
	else{
		//如果到了蛇尾，并且蛇头吃到了食物，则在尾巴处新建节点
		if(flag == 1){
			newp = (struct point *)malloc(sizeof(struct point));
			newp->next = NULL;
			newp->x = temp->x;
			newp->y = temp->y;
			newp->dir = temp->dir;
			temp->next = newp;
		}
	}
	//当前节点变成目标（上一个节点）状态
	temp->dir = dir;
	temp->x = x;
	temp->y = y;
	return;
}
void show(int dis[][N]){
	int i = 0;
	int j = 0;
	system("cls");
	for(i = 0;i<N;i++){
		if(i == 0){
			printf("方向键控制上下左右\n");
			for(j = 0;j<N+2;j++){
				printf("■");
			}
			printf("\n");
		}
		for(j = 0;j<N;j++){
			if(j == 0){
				printf("■");
			}
			if(dis[i][j] == 0){
				printf("  ");
			}
			else if(dis[i][j] == 1){
				printf("■");
			}
			else{
				printf("★");
			}
			if(j == N-1){
				printf("■");
			}
		}
		printf("\n");
		if(i == N-1){
			for(j = 0;j<N+2;j++){
				printf("■");
			}
		}
	}
	return;
}
