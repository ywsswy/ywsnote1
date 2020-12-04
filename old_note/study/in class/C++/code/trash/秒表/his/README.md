#include <stdio.h>  
#include <conio.h> 
#include <windows.h> 
#include <stdlib.h>  
struct tm     //定义时间结构体，包括时分秒和10毫秒 
{ 
int hours,minutes,seconds; 
int hscd; 
}time,tmp,total;    //time用以计时显示，tmp用以存储上一阶段时间，total记总时间 
int cnt; 
FILE* fout; 
//每次调用update函数，相当于时间过了10ms 
void update(struct tm *t)    
{ 
(*t).hscd++;    //10ms单位时间加1  
cnt++; 
if ((*t).hscd==100)   //计时满1s，进位 
{  
  (*t).hscd=0;  
  (*t).seconds++;  
}  
if ((*t).seconds==60)   //计时满一分，进位 
{  
  (*t).seconds=0;  
  (*t).minutes++;  
}  
if ((*t).minutes==60)        //计时满一小时，进位 
{  
  (*t).minutes=0;  
  (*t).hours++;  
}  
if((*t).hours==24) (*t).hours=0;  
//delay(); 
Sleep(10);  //Sleep是windows提供的函数，作用是暂停程序，单位毫秒，所以此处暂停10ms 
}  
void display(struct tm *t)  
{  
//此处输出计时结果，\r为回车不换行，既一直在同一行更新时间 
printf("%d:",(*t).hours);  
printf("%d:",(*t).minutes);  
printf("%d:",(*t).seconds); 
printf("%d\r",(*t).hscd);  
//printf("Now, press ‘e’ key to stop the clock…");  
}  
void time_init()  //初始化时间 
{ 
time.hours=time.minutes=time.seconds=time.hscd=0; 
} 
void get_total()   //计算总时间 
{ 
total.hscd = cnt % 100; 
cnt /= 100; 
total.seconds = cnt % 60; 
cnt /= 60; 
total.minutes = cnt % 60; 
cnt /= 60; 
total.hours = cnt; 
} 
int main()  
{ 
char m;   
time_init(); 
cnt = 0; 
fout =  fopen("timeout.txt","w");    
printf("按回车键开始计时!\n"); 
while(1) 
{ 
  m = getch(); 
  if(m != ‘\r’)     //读入一个输入，如果是回车，那么跳出次循环 
   printf("输入错误，仅能输入回车键!\n"); 
  else 
   break; 
} 
printf("已经开始计时，但是你可以按回车键以分段计时!\n"); 
while(1) 
{ 
  if(kbhit())    //此处检查是否有键盘输入 
  { 
   m=getch();  
   if(m == ‘\r’)     //如果等于回车，那么计时结束，跳出循环 
    break; 
   else if(m == ‘ ‘)  //如果等于空格，显示此次计时，初始化计时器 
   { 
    tmp = time;      //记录上一段计时器结果 
    fprintf(fout,"%d:%d:%d:%d\n",tmp.hours,tmp.minutes,tmp.seconds,tmp.hscd); //写入文件 
    time_init(); 
    printf("\n"); 
   } 
   else 
   { 
    printf("输入错误，仅支持输入回车键或者空格键!\n"); 
   }  
  } 
  update(&time);     //更新计时器 
  display(&time);    //显示计时器时间 
} 
tmp = time;       //输出最后一次即使结果，写入文件 
fprintf(fout,"%d:%d:%d:%d\n",tmp.hours,tmp.minutes,tmp.seconds,tmp.hscd); 
get_total();      //计算总的时间，显示，并写入文件 
printf("\n总时间:%d:%d:%d:%d\n",total.hours,total.minutes,total.seconds,total.hscd); 
fprintf(fout,"统计时间:%d:%d:%d:%d\n",total.hours,total.minutes,total.seconds,total.hscd); 
fclose(fout); 
printf("已经保存到当前目录下的timeout.txt文件中按任意键结束!"); 
getch();  
}
