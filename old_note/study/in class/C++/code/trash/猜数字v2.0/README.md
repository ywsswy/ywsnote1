#include<stdlib.h>
#include<time.h>
#include<stdio.h>
int main(){
	int a = 0;//随机数
	int b = 0;//猜的数
	char c = 0;
	int n = 0;//获取scanf返回值，判断数据输入是否成功
	//生成随机数
	srand(time(NULL));
	a = rand()%10000;
	while(1){
		//猜数字
		printf("请猜一个数\n");
		n = scanf("%d",&b);
		if(n == 0){
			while(1){
				scanf("%c",&c);
				if(c == '\n'){
					break;
				}
			}
			continue;
		}
		//判断正误
		if(a == b){
			printf("你猜对了，666\n");
			break;
		}
		else if(a > b){
			printf("你输入的比正确的数小，请重新输入\n");
		}
		else{
			printf("你输入的比正确的数大，请重新输入\n");
		}	
	}
	return 0;
}
