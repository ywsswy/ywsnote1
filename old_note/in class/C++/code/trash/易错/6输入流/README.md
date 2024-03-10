从键盘任意输入两个符号各异的整数，直到输入的两个整数满足要求为止，然后打印这两个数。
输入格式: "%d,%d"
输出格式：
输入提示信息："Input x1, x2:\n"
输出： "x1=%d,x2=%d\n"
#include <stdio.h>
int main()
{
    int x1 = 0;
	int x2 = 0;
	int n1 = 0;
	int n2 = 0;
	char c = 0;
    do{
		printf("Input x1, x2:\n");
		n1 = scanf("%d,%d", &x1, &x2);
		do{
			n2 = scanf("%c",&c);
		}while(c != '\n' && n2 != EOF);
    }while (x1 * x2 >= 0 || n1 != 2);
    printf("x1=%d,x2=%d\n", x1, x2);
    return 0;
}
