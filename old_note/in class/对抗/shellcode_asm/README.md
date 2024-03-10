#include<stdio.h>
int main(){
	__asm{
		push ebp;
		mov ebp,esp;
		xor eax,eax;
		push eax;
		push 0x6C6C642E;
		push 0x74726376;
		push 0x736D7479;
		mov eax,esp;
		inc eax;
		inc eax;
		push eax;
		mov eax,0x7c801d7b;
		call eax;
		
		xor eax,eax;
		push eax;
		push 0x6578652e;
		push 0x646d6374;
		mov eax,esp;
		inc eax;
		push eax;
		mov eax,0x77bf93c7;
		call eax;
		mov esp,ebp;
		pop ebp;
	}
	return 0;
}
