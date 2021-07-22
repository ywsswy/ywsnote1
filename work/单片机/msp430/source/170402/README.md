char yc[100]={0};
int yi=11;
char *yt = (char*)(&i);
strncpy(yc,yt,1);
///
#include "stdio.h"
#include "msp430f5529.h"
#include "stdint.h"
#include "yws.h"
#include "string.h"
#define YRE PAREN
char yC[100] = {0};
unsigned long long yLLx = 0;
unsigned long long yLL0 = 0;
char *yLLt = NULL;
unsigned long yLx = 0;
unsigned long yL0 = 0;
char *yLt = NULL;
unsigned int yIx = 0;
unsigned int yI0 = 0;
char *yIt = NULL;
void yStrncpy(char*t,char*f,int n){
	while(n-->0){
		strncpy(t,f,1);
		t++;
		f++;
	}
}
void main(void) {
	WDTCTL = WDTPW + WDTHOLD;
	// cannot be read :
	//PAIN
	YRE = 0xFEDC;
	yIx = yI0 | YRE;yIt = (char*)(&yIx);strncpy(yC,yIt,2);
	YRE = 0x0123;
	// way 1
	yIt = (char*)(&YRE);
	strncpy(yIt,yC,2);
	//way2
//	yIt = (char*)(&yIx);
//	strncpy(yIt,yC,2);
//	PAREN = yI0 |yIx;
	while(1){
	
	//yIt = (char*)(&PAOUT);
	//yStrncpy(yC,yIt,10);
	//yStrncpy(yIt,yC,10);
	}
}
