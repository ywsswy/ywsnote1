#include<stdlib.h>
#include<time.h>
#include<stdio.h>
int main(){
	int a = 0;//随机数
	int b = 0;//猜的数
	//生成随机数
	srand(time(NULL));
	a = rand()%10000;
	//猜数字
	while(1){
		printf("请猜一个数\n");
		scanf("%d",&b);
		if(a == b){
			break;
		}
		else if(a > b){
			printf("你输入的比正确的数小，请重新输入\n");
		}
		else{
			printf("你输入的比正确的数大，请重新输入\n");
		}	
	}
	printf("你猜对了，666\n");
	return 0;
}
