#include <REGX52.H>	 
#include "Yboard.h"
 
char gt;
char gh = 0;
char gm = 0;
char gs = 0;
int pres = 0;
char speaflag = 0;
void yShow(){
	char ystr[20] = "";
	ystr[0] = gh/10+'0';
	ystr[1] = gh%10+'0';
	ystr[2] = ':';
	ystr[3] = gm/10+'0';
	ystr[4] = gm%10+'0';
	ystr[5] = ':';
	ystr[6] = gs/10+'0';
	ystr[7] = gs%10+'0';
	ystr[8] = 0;
	yStr_1602(YFIRSTLINE1602,ystr);
}	 
void yCal(){
	if((int)gt0 != pres){
		gs++;
		pres = (int)gt0;
		if(gs == 60){
			gs = 0;
			gm++;
			if(gm == 60){
				gm = 0;
				gh++;
				if(gh == 24){
					gh = 0;
				}
			}
		}
	}
}
void main(){
	yBoard_init();
	y1602_init(); 
	yT0_init();	  
#ifdef MODgt11
	yUSRT1_init()
#endif
#ifdef MODgt12
	yT1_init();
	yT1_disa();
#endif
	while(1){
		yCal();
		if(YUPKEY==YKEY_PRESS){
			yShow();
		}
	}
}

