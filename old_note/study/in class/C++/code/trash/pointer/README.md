#include <stdio.h>
int main(){
	int a = 0;
	int b[3] = {3,2,1};
	int c[3][4] = {{11,12},{21,22},{31,32}};
	int *p1 = &a;
	int **p4 = &p1;
	int *p2 = &b;
	int *p3 = &c;
	int (*p22)[3] = &b;
	int (*p32)[4] = &c;
	return 0;
}
