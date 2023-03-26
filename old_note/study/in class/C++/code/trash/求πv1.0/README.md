#include <stdio.h>
#include <math.h>
#include <string.h>
#include <stdlib.h>
int main()
{
	char s1[20] = "%.";
	int precision = 0;
	char s2[20] = "";
	char *s3 = "lf\n";
	printf("穷人版求π值，你想精确到多少位？\n");
	scanf("%d",&precision);
	itoa(precision,s2,10);
	strcat(s1,s2);
	strcat(s1,s3);
	printf(s1,acos(-1));
    return 0;
}
