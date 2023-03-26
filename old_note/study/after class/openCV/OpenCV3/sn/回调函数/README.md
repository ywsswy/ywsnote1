//就是一种函数指针调用
#include <stdio.h>
 
void printWelcome(int len)
{
       printf("欢迎欢迎 -- %d\n", len);
}
 
void printGoodbye(int len)
{
       printf("送客送客 -- %d\n", len);
}
 
void callback(int times, void (* print)(int))
{
       int i;
       for (i = 0; i < times; ++i)
       {
              print(i);
       }
       printf("/n我不知道你是迎客还是送客!\n\n");
}
void main(void)
{
       callback(10, printWelcome);
       callback(10, printGoodbye);
       printWelcome(5);
}
