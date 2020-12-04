#include<stdio.h>
int main(){
    char word = 0;
    int n = 0;
    do{
        n = scanf("%c",&word);//EOF -1
        if(word == '\n')
        {
            printf(" ");
        }
        else{
            printf("%c",word);
        }
    }
    while(n == 1);
    return 0;
}

