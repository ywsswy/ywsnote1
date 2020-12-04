#if 0 //闹钟
		yCal();
		if(YUPKEY==YKEY_PRESS){
			yTestDis();
		}
		if(YDOWNKEY==YKEY_PRESS){
			while(YUPKEY != YKEY_PRESS){ 
				yTestDis();
				if(YDOWNKEY==YKEY_PRESS && (int)gt0 != pres){
					gh++;
					gh=gh%24;
				}	 
				yCal();
			}
			while(YDOWNKEY != YKEY_PRESS){ 
				yTestDis();
				if(YUPKEY==YKEY_PRESS && (int)gt0 != pres){
					gm++;
					gm=gm%60;
				}		
				yCal();
			}
			while(YUPKEY != YKEY_PRESS){ 
				yTestDis();
				if(YDOWNKEY==YKEY_PRESS && (int)gt0 != pres){
					dh++;
					dh=dh%24;
				}	
				yCal();
			}
			while(YDOWNKEY != YKEY_PRESS){ 
				yTestDis();
				if(YUPKEY==YKEY_PRESS && (int)gt0 != pres){
					dm++;
					dm=dm%60;
				}	
				yCal();
			}
			while(YUPKEY != YKEY_PRESS);
		}	
	}
#endif
