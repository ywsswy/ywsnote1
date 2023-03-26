#include<stdio.h>
int main()
{
    int a[4][2]={{1,2},{3,4},{5,6},{7,8}};
    int i,k;
    printf("Array a:\n");
    for(i=0;i<=3;i++)
    {
        for(k=0;k<=1;k++)
        {
            printf("%4d",a[i][k]);
        }
        printf("\n");
    }
    printf("Array b:\n");
    for(i=0;i<=1;i++)
    {
        for(k=0;k<=3;k++);
        {
            printf("%4d",a[k][i]);
        }
        printf("\n");
    }
}

