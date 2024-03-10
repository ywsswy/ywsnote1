#include<stdio.h>
#include<string.h>
#define N 80
void Inverse(char str[])
{
    int i,temp,j,k,l;
    k=0;
    for(i=0;str[i]!='\0';i++)
    {
        k++;
    }
    for (l=0;l<k/2;l++)
    {
        temp=str[l];
        str[l]=str[k-l];
        str[k-l]=temp;
 
    }
    printf("Inversed results:%s\n",str);
}
main()
{
    char str[N];
    printf("Input a string:\n");
   scanf("%s",str);
    Inverse(str);
 
}
