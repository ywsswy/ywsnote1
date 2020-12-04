#include<stdio.h>
#include<windows.h>
typedef void(*MYPROC)(void);
int main(int argc,char*argv[]){
	HINSTANCE LibHandle;
    MYPROC ProcAdd;
    LibHandle=LoadLibrary("kernel32.dll");
    ProcAdd=(MYPROC)GetProcAddress(LibHandle,"LoadLibraryA");
    printf("//x%x\n",ProcAdd);
    LibHandle=LoadLibrary("msvcrt.dll");
    ProcAdd=(MYPROC)GetProcAddress(LibHandle,"system");
    printf("//x%x\n",ProcAdd);
	return 0;
}
