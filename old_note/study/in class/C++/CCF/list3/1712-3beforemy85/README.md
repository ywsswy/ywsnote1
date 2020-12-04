#define YLOFI
//#define YDELO
#include<iostream>
#include<iomanip>
#include<string>
#include<sstream>
#include<map>
#include<list>
#include<vector>
#include<algorithm>
#include<cmath>
using namespace std;
int vv[20][5][60] = {0};//每一条任务 六个属性 每个值是否符合要求 
#ifdef YDELO
#include "assert.h"
int ydelon = 0;
//Print
template<typename T>
bool yPrint(const string &info,const T &x,int n = 0,bool clr = true){
	if(clr){
		system("cls");
	}
	cout << endl << "\\**************************" << endl << info << endl;
	ydelon = n;
	if(ydelon >= 0){
		cout << x;
	}
	else{
		return false;
	}
	cout << endl << "**************************\\" << endl;
	return true;
}
//list & map & vector
template<typename T,typename S>
ostream &operator<<(ostream &os,const pair<T,S> &it){
	return os << it.first << " " << it.second;
}
template<typename T,typename S>
ostream &operator<<(ostream &os,const map<T,S> &st){
	int n = ydelon==0 ? st.size() : ydelon,i = 0;
	os << " size=" << st.size() << " show=" << n << endl;
	for(typename map<T,S>::const_iterator it = st.begin();it!=st.end();it++){
		os << i << " " << *it;
		i++;
		if(i >= n){
			break;
		}
		else{
			os << endl;
		}
	}
	return os;
}
template<typename T>
ostream &operator<<(ostream &os,const list<T> &st){
	int n = ydelon==0 ? st.size() : ydelon,i = 0;
	os << " size=" << st.size() << " show=" << n << endl;
	for(typename list<T>::const_iterator it = st.begin();it!=st.end();it++){
		os << i << " " << *it;
		i++;
		if(i >= n){
			break;
		}
		else{
			os << endl;
		}
	}
	return os;
}
template<typename T>
ostream &operator<<(ostream &os,const vector<T> &st){
	int n = ydelon==0 ? st.size() : ydelon,i = 0;
	os << " size=" << st.size() << " show=" << n << endl;
	for(typename vector<T>::const_iterator it = st.begin();it!=st.end();it++){
		os << i << " " << *it;
		i++;
		if(i >= n){
			break;
		}
		else{
			os << endl;
		}
	}
	return os;
}
#endif
int gbig[13] = {0,31,0,31,30,31,30,31,31,30,31,30,31};
//                 1    3    5     7   8     10    12
int yCaile(int year,int month,int day){
	if(month == 1 || month == 2){
		month += 12;
		year--;
	}
	int century = year/100;
	year = year%100;
	int buf = year + year/4 - 2*century + century/4 + 26*(month+1)/10 + day -1;
	while(buf < 0){
		buf += 7;
	}
	return buf%7;
}
bool yRun(int year){
	if(year % 400 == 0){
		return true;
	}
	else if(year % 100 == 0){
		return false;
	}
	else if(year % 4 == 0){
		return true;
	}
	else{
		return false;
	}
}
int getBig(int year,int month){
	if(month != 2){
		return gbig[month]+1;
	}
	else{
		if(yRun(year)){
			return 29+1;
		}
		else{
			return 28+1;
		}
	}
}
int det[5][2] = {0,59,
0,23,
1,31,
1,12,
0,6};//每个属性的需要考虑的范围分钟[0,59]，小时[0,23]。。。 
//						值的范围分钟[0,59]，小时[0,23]，天[1,28-31] ，月[1,12] 
//						
void yDe1(string &inp,int ii,int typet){
	int type = 0;//1 -  2,
	int lo1 = inp.find_first_of("*");
	
	#ifdef YDELO
	assert(yPrint("string inp",inp,0,false));
	#endif
	if(lo1 != -1){
		int st1 = 0;
		int en1 = 0;
		st1 = det[typet-1][0];
		en1 = det[typet-1][1];
		for(int i = st1;i<=en1;i++){
			vv[ii][typet-1][i] = 1;
		}
		inp = "";
	}
	else{
		int lo2 = 0;
		int lo3 = 0;
		int lom = 0;
		while(1){
			lo3 = inp.find_first_of("-");
			lo2 = inp.find_first_of(",");
			if(lo2 == -1 && lo3 == -1){
				break;
			}
			if(lo3 == -1){
				lom = lo2;
			}
			else if(lo2 == -1){
				lom = lo3;
			}
			else{
				lom = min(lo3,lo2);
			}
			if(lom == lo3){
				type = 1;
			}
			else{
				type = 2;
			}
			int left = 0;
			string bufs = inp.substr(0,lom);
			inp = inp.substr(lom+1);
			stringstream ss;
			ss << bufs;
			ss >> left;
			if(type == 2){
				vv[ii][typet-1][left] = 1;
			}
			else{
				int bufll = inp.find_first_not_of("0123456789");
				string bufss;
				if(bufll == -1){
					bufss = inp;
					inp = "";
				}
				else{
					bufss = inp.substr(0,bufll);
					inp = inp.substr(bufll+1);
				}
				int right = 0;
				stringstream ss2;
				ss2 << bufss;
				ss2 >> right;
				for(int i = left;i<=right;i++){
					vv[ii][typet-1][i] = 1;
				}
			}
		}
	}
	if(inp.length() == 0){
		return;
	}
	else{
		int final = 0;
		stringstream ss3;
		ss3 << inp;
		ss3 >> final;
		vv[ii][typet-1][final] = 1;
	}
} 
void yCh(string &str){
	int lo = 0;
	lo = str.find("Jan");
	if(lo != -1)
	str = str.replace(lo,3,"1");
	lo = str.find("Feb");
	if(lo != -1)
	str = str.replace(lo,3,"2");
	lo = str.find("Mar");
	if(lo != -1)
	str = str.replace(lo,3,"3");
	lo = str.find("Apr");
	if(lo != -1)
	str = str.replace(lo,3,"4");
	lo = str.find("May");
	if(lo != -1)
	str = str.replace(lo,3,"5");
	lo = str.find("Jun");
	if(lo != -1)
	str = str.replace(lo,3,"6");
	lo = str.find("Jul");
	if(lo != -1)
	str = str.replace(lo,3,"7");
	lo = str.find("Aug");
	if(lo != -1)
	str = str.replace(lo,3,"8");
	lo = str.find("Sep");
	if(lo != -1)
	str = str.replace(lo,3,"9");
	lo = str.find("Oct");
	if(lo != -1)
	str = str.replace(lo,3,"10");
	lo = str.find("Nov");
	if(lo != -1)
	str = str.replace(lo,3,"11");
	lo = str.find("Dec");
	if(lo != -1)
	str = str.replace(lo,3,"12");
	lo = str.find("Sun");
	if(lo != -1)
	str = str.replace(lo,3,"0");
	lo = str.find("Mon");
	if(lo != -1)
	str = str.replace(lo,3,"1");
	lo = str.find("Tue");
	if(lo != -1)
	str = str.replace(lo,3,"2");
	lo = str.find("Wed");
	if(lo != -1)
	str = str.replace(lo,3,"3");
	lo = str.find("Thu");
	if(lo != -1)
	str = str.replace(lo,3,"4");
	lo = str.find("Fri");
	if(lo != -1)
	str = str.replace(lo,3,"5");
	lo = str.find("Sat");
	if(lo != -1)
	str = str.replace(lo,3,"6");
}
void showArray(int v[][5][60],int n){
	for(int i = 0;i<n;i++){
		for(int j = 0;j<5;j++){
			cout << "v[" << i << "][" << j << "]:" << endl;
			for(int k = 0;k<60;k++){
				cout << v[i][j][k] << " ";
			}
			cout << endl;
		}
		cout << endl;
	}
}
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	#ifdef YLOFI
	freopen("yin1.txt","r",stdin);
	#endif
	#ifdef YDELO
	assert(1);
	#endif
	int gn = 0;//任务数量 
	string gs;
	string ge;
	cin >> gn;
	cin >> gs;
	cin >> ge;
	int gsy = 0;
	gsy = gs[0]-'0';
	gsy = gsy*10+gs[1]-'0';
	gsy = gsy*10+gs[2]-'0';
	gsy = gsy*10+gs[3]-'0';
	int gey = 0;
	gey = ge[0]-'0';
	gey = gey*10+ge[1]-'0';
	gey = gey*10+ge[2]-'0';
	gey = gey*10+ge[3]-'0';
	int gso = 0;
	gso = gs[4]-'0';
	gso = gso*10+gs[5]-'0';
	int geo = 0;
	geo = ge[4]-'0';
	geo = geo*10+ge[5]-'0';
	int gsd = 0;
	gsd = gs[6]-'0';
	gsd = gsd*10+gs[7]-'0';
	int ged = 0;
	ged = ge[6]-'0';
	ged = ged*10+ge[7]-'0';
	int gsh = 0;
	gsh = gs[8]-'0';
	gsh = gsh*10+gs[9]-'0';
	int geh = 0;
	geh = ge[8]-'0';
	geh = geh*10+ge[9]-'0';
	int gsm = 0;
	gsm = gs[10]-'0';
	gsm = gsm*10+gs[11]-'0';
	int gem = 0;
	gem = ge[10]-'0';
	gem = gem*10+ge[11]-'0';
	int gss = 0;//星期日是0 
	gss = yCaile(gsy,gso,gsd);
	int bigday = 0;//当前月份的天数，月份改的时候更新
	bigday = getBig(gsy,gso);//大一哦 
	//
	vector<string> vinfo;// 任务信息 
	vector<int> vpri;//1 上一次输出了，未写 
	for(int i = 0;i<gn;i++){
		//每一条任务 
		vpri.push_back(1);
		
		string bufs1;
		cin >> bufs1;
		yDe1(bufs1,i,1);//分钟 
		cin >> bufs1;
		yDe1(bufs1,i,2);//小时
		cin >> bufs1;
		yDe1(bufs1,i,3);//天 
		cin >> bufs1;
		yCh(bufs1);
		yDe1(bufs1,i,4);//月
		cin >> bufs1;
		yCh(bufs1);
		#ifdef YDELO
		assert(yPrint("string bufs1,2",bufs1,0,false));
		#endif
		yDe1(bufs1,i,5);//星期
		
		cin >> bufs1;
		vinfo.push_back(bufs1);
	}
	#ifdef YDELO
	//assert(yPrint("bigday",bigday,0,false));
	showArray(vv,3);
	#endif
	//状态改变标志
	int gcs = 1;//星期 
	int gco = 1;
	int gcd = 1;
	int gch = 1;
	int gcm = 1; //肯定变 
	//上一次是否输出
	//取决于哪些状态 
	while(true){
		//cout 
		//相等则退出（加速-次要，判断状态改变）
		if(gsm == gem && gsh == geh && gsd == ged && gso == geo && gsy == gey){
			break;
		}
		for(int i = 0;i<gn;i++){
			if(vv[i][0][gsm] == 1
			&&
			vv[i][1][gsh] == 1
			&&
			vv[i][2][gsd] == 1
			&&
			vv[i][3][gso] == 1
			&& 
			vv[i][4][gss] == 1){
				cout << setfill('0') << setw(4) << gsy
				<< setw(2) << gso
				<< setw(2)<< gsd
				<< setw(2)<< gsh
				<< setw(2)<< gsm
				<< " " << vinfo[i] <<  endl;
			}
		}
		//计算下一分钟状态 
		gsm++;
		if(gsm == 60){
			gsh++;
			gch = 1;
			gsm = 0;
			if(gsh == 24){
				gsd++;
				gss++;
				if(gss == 7){
					gss = 0;
				}
				gcd = 1;
				gcs = 1;
				gsh = 0;
				if(gsd == bigday){
					gso++;
					gco = 1;
					gsd = 1; 
					if(gso == 13){
						gsy++;
						gso = 1; 
					}
					bigday = getBig(gsy,gso);
				}
				else{
					gco = 0;
				}
			}
			else{
				gcs = 0;
				gcd = 0;
			}
		}
		else{
			gch = 0;
		}
	}
	return 0;
}
