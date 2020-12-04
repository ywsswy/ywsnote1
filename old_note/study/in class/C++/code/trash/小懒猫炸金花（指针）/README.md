//模拟五人炸金花的发牌（数组实现）
#include<stdio.h>
#include<time.h>
#include<stdlib.h>
char suit[4] = {3,4,5,6};//四种花色：红桃、方块、梅花、黑桃
char face[13] = {'2','3','4','5','6','7','8','9','@','J','Q','K','A'};//十三种牌型
void chushihua(char *pai){//初始化，按顺序理好牌
	int i = 0;
	int j = 0;
	for(i = 0;i<4;i++){
		for(j = 0;j<13;j++){
			*(pai+(i*13+j)*2+0) = *(suit+i);
			*(pai+(i*13+j)*2+1) = *(face+j);
		}
	}
}
void xipai(char (*pai)[13][2]){//洗牌，打乱顺序
	int i = 0;
	int j = 0;
	int m = 0;
	char temp = 0;
	srand(time(NULL));
	for(m = 0;m<15;m++){
		i = rand()%4;
		j = rand()%13;
		temp = *(*(*(pai+i)+j)+0);
		*(*(*(pai+i)+j)+0) = *(*(*(pai+0)+m)+0);
		*(*(*(pai+0)+m)+0) = temp;
		temp = *(*(*(pai+i)+j)+1);
		*(*(*(pai+i)+j)+1) = *(*(*(pai+0)+m)+1);
		*(*(*(pai+0)+m)+1) = temp;
	}
}
void fapai(char *pai){//给五个人发牌
	int i = 0;
	int j = 0;
	int m = 0;
	printf("甲\t乙\t丙\t丁\t戊\n");
	for(i = 0,m = 0;i<3;i++){
		for(j = 0;j<5;j++,m++){
			printf("%c",*(pai+m*2+0));
			switch(*(pai+m*2+1)){
			case '@':
				printf("%d\t",10);
				break;
			default:
				printf("%c\t",*(pai+m*2+1));
				break;
			}
		}
		printf("\n");
	}
}
int main(){
	char pai[4][13][2];
	chushihua(pai);//初始化
	xipai(pai);//洗牌
	fapai(pai);//发牌
	return 0;
}

