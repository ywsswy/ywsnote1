int yZeller(int year, int month, int day){
	if (month == 1 || month == 2){
		month += 12;
		year--;
	}
	int century = year / 100;
	year = year % 100;
	int buf = year + year / 4 - 2 * century + century / 4 + 26 * (month + 1) / 10 + day - 1;
	while (buf < 0){
		buf += 7;
	}
	return buf % 7;
}
#include<stdio.h>
//蔡勒公式 返回值星期 0:sunday 1~6 monday~saturday
int main()
{
	int y, m, d;//这一天的年月日
	int tw = 0;//这个月28日的星期
	int nw = 0;//下个月1日的星期
	int dn = 0;//这个月的天数
	scanf("%d%d%d", &y, &m, &d);
	nw = yZeller(y + m/12, (m)%12+1, 1);
	tw = yZeller(y, m, 28);
	dn = 28 - 1 + (nw - tw + 7) % 7;
	if (d == dn){
		d = 1;
		m++;
		if (m > 12){
			m = 1;
			y++;
		}
	}
	else{
		d++;
	}
	printf("下一天是%04d年%02d月%02d日", y, m, d);
	return 0;
}
