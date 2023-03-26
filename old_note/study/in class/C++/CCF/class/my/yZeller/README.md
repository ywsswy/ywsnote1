int yZeller(int year,int month,int day){
	if(month == 1 || month == 2){
		month += 12;
		year--;
	}
	int century = year/100;
	year = year%100;
	int buf = year + year/4 - 2*century + century/4 + 26*(month+1)/10 + day - 1;
	while(buf < 0){
		buf += 7;
	}
	return buf%7;
}
//1582年10月4日后
//others
string getWeekday(string year,string month,string day)
{
    int y=stoi_x(year),m=stoi_x(month),d=stoi_x(day);
    int by=1970,countday=0;
    while(by<y)
    {
        countday+=(isleapyear(by))?366:365;
        ++by; 
    }
    for(int i=1;i<m;++i) countday+=mtharray[i];
    countday+=d-1;
    return "0"+to_string_x((4+countday%7)%7);
}
