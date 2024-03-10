#include<stdio.h>
/*
函数功能：洗澡
输入参数：
    name：人名
        1：孙仲达
        2：李玉菲
        3：张烨
    shuiwen：水温
        1：热水
        0：冷水
    yifu：衣服
        1：男装
        0：女装
返回值：无
*/
void xizao(int name,int shuiwen,int yifu){
    if(name == 1){
        printf("孙仲达洗澡：\n");
    }
    else if(name == 2){
        printf("李玉菲洗澡：\n");
    }
    else{
        printf("张烨洗澡：\n");
    }
    //脱衣服
    printf("1.脱衣服\n");
    //用水洗
    if(shuiwen == 0){
        printf("2.用冷水洗澡\n");
    }
    else{
        printf("2.用热水洗澡\n");
    }
    //穿衣服
    if(yifu == 0){
        printf("3.穿了女装\n");
    }
    else{
        printf("3.穿了男装\n");
    }
    return;
}
int main(){
    int sunshuiwen = 0;//0：冷水 1：热水
    int sunyifu = 0;//0：女装 1：男装
    int lishuiwen = 1;//0：冷水 1：热水
    int liyifu = 0;//0：女装 1：男装
    int zhangshuiwen = 1;//0：冷水 1：热水
    int zhangyifu = 0;//0：女装 1：男装
    xizao(1,sunshuiwen,sunyifu);
    xizao(2,lishuiwen,liyifu);
    xizao(3,zhangshuiwen,zhangyifu);
	return 0;
}

