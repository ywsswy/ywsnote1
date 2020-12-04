#include<stdio.h>
int main(){
	int i = 1;
	int j = 0;
	int n = 0;
	int suc = 0;
	int flag = 1;
	printf("Input n(n<=5):\n");
	scanf("%d",&n);
	i = 0;
	for(i = 0;;i++){
		suc = 0;
		j = i;
		while( (j-1)%5 == 0 && (j-1)/5 > 0){
			suc ++;
			j = j/5*4;
			if(suc == n){
				printf("y=%d\n",i);
				flag = 0;
				break;
			}
		}
		if(flag == 0){
			break;
		}
	}
	return 0;
}
