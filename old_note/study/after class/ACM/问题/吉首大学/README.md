int gcd(int a, int b){ return a == 0 ? b : gcd(b % a, a); }
#include<stdio.h>
int main(){
	int n = 0;
	double s = 0;
	int t = 0;
	double h = 0;
	while(scanf("%d",&n)!=EOF){
		h = (double)n;
		s = h*(h+1)*(2*h+1)/3;
		t = ((long long)(0.5+s))%10007;
		printf("%d\n",t);
	}
	return 0;
}
