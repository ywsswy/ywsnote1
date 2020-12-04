#include<stdio.h>
int main(){
	char c;
	freopen("in.txt","r",stdin);
	while(~scanf("%c",&c)){
		if(0x20 != c && 0x0a != c){
			printf("%c",c);
		}
	}
	return 0;
}
