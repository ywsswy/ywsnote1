int main(){
	char s[18] = "#################";
	char index[] ="0123456789";
	char i = 0,j = 0;
	float num = 0.00 123456789012345;
	char loch = 127;
	char locd = 127;
	float temp2 = 1.0;
	float temp = 100000000000000.0;
	for(i = 15;i>-14 &&( i>=0 || (i<0 && (loch-i)<6));i--,temp = temp/10.0){
		
		if((int)(num/temp)!=0 && loch == 127){
			loch = i;
		}
		if(loch != 127){
			if(i == 0){
				s[loch] = '.';
			}
			if(i > 0){
				s[loch-i]=index[(int)(num/temp)];
				for(j = 1,temp2 = 1.0;j<i;j++){
					temp2 = temp2*10.0;
				}
				num=num-(float)(s[loch-i]-'0')*temp2;
			}
			else{
				s[loch-i+1]=index[(int)(num/temp)];
				for(j = 0,temp2 = 1.0;j>=i;j--){
					temp2 = temp2/10.0;
				}
				num=num-(float)(s[loch-i+1]-'0')*temp2;
			}
		}
	}
	printf("%d\n",loch);
	printf("%s\n",s);
}
