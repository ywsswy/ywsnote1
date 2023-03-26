/*
A+B和C (15)
给定区间[-231, 231]内的3个整数A、B和C，请判断A+B是否大于C
*/
//gcc
#include<stdio.h>
//#include<iostream> //No such file or directory in gcc
#if 0 //localhost
	#define long_long __int64
	int main(){
		int n = 0,i = 0;
		long_long a = 0,b = 0,c = 0;
#else
	int main(){
		int n = 0,i = 0;
		long long a = 0,b = 0,c = 0;
	
#endif
	scanf("%d",&n);
	for(i = 0;i<n;i++){
		scanf("%lld%lld%lld",&a,&b,&c);
		if(a+b-c > 0)
			printf("Case #%d: true\n",i+1);
		else
			printf("Case #%d: false\n",i+1);
	}
	return 0;
}
//g++
#include<stdio.h>
#include<iostream> //exist in g++
#if 0 //localhost
	#define long_long __int64
	int main(){
		int n = 0,i = 0;
		long_long a = 0,b = 0,c = 0;
#else
	int main(){
		int n = 0,i = 0;
		long long a = 0,b = 0,c = 0;
	
#endif
	scanf("%d",&n);
	for(i = 0;i<n;i++){
		scanf("%lld%lld%lld",&a,&b,&c);
		if(a+b-c > 0)
			printf("Case #%d: true\n",i+1);
		else
			printf("Case #%d: false\n",i+1);
	}
	return 0;
}
