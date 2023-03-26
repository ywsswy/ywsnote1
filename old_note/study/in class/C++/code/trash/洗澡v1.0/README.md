#include<stdio.h>
int main(){
    int sunshuiwen = 0;//0：冷水 1：热水
    int sunyifu = 0;//0：女装 1：男装
    int lishuiwen = 1;//0：冷水 1：热水
    int liyifu = 0;//0：女装 1：男装
    printf("孙仲达洗澡：\n");
    //脱衣服
    printf("1.脱衣服\n");
    //用水洗
    if(sunshuiwen == 0){
        printf("2.用冷水洗澡\n");
    }
    else{
        printf("2.用热水洗澡\n");
    }
    //穿衣服
    if(sunyifu == 0){
        printf("3.穿了女装\n");
    }
    else{
        printf("3.穿了男装\n");
    }
    printf("李玉菲洗澡：\n");
    //脱衣服
    printf("1.脱衣服\n");
    //用水洗
    if(lishuiwen == 0){
        printf("2.用冷水洗澡\n");
    }
    else{
        printf("2.用热水洗澡\n");
    }
    //穿衣服
    if(liyifu == 0){
        printf("2.穿了女装\n");
    }
    else{
        printf("2.穿了男装\n");
    }
	return 0;
}

