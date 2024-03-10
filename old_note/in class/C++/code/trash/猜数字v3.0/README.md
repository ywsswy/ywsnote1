#include<stdlib.h>
#include<time.h>
#include<stdio.h>
/*
函数功能：判断两个数是否相等
输入参数：
    a：生成的数
    b：猜的数
返回值：
    0：不相等
    1：相等
*/
int judge(int a ,int b){
    if(a>b){
        printf("你输入的比正确的数小，请重新输入\n");
        return 0;
    }
    else if(a==b){
        printf("你猜对了，666\n");
        return 1;
    }
    else{
        printf("你输入的比正确的数大，请重新输入\n");
        return 0;
    }
}
/*
函数功能：获取一个输入数据
输入参数：无
返回参数：获取到的数据
*/
int get(){
    int b = 0;
	char c = 0;
	int n = 0;//获取scanf返回值，判断数据输入是否成功
	while(1){
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
        else{
            return b;
        }
	}
}
int main(){
	int a = 0;//随机数
	int b = 0;//猜的数
	//生成随机数
	srand(time(NULL));
	a = rand()%10000;
	while(1){
        //猜数字
        b = get();
		//判断
		if(judge(a,b) == 1){
		    break;
		}
	}
	return 0;
}

