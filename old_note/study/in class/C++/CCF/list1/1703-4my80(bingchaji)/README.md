#define YLOFI
#define YDELO
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
struct yc3i{
	int from;//0s
	int to;//0s
	int len;
};
struct yc2i{
	int fa;//žž/žùœÚµã 
	int rank;//²ãŽÎ/µÈŒ¶ 
};
bool operator<(const yc3i &x,const yc3i &y){
	return x.len <= y.len;
} 
#ifdef YDELO
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
//×Ô¶šÒå
ostream &operator<<(ostream &os,const yc2i &st){
	os << "fa=" << st.fa << ",rank=" << st.rank;
	return os;
}
ostream &operator<<(ostream &os,const yc3i &st){
	os << "from=" << st.from << ",to=" << st.to << ",len=" << st.len;
	return os;
}
//·ÇÊý×é
template<typename T>
bool yPrint(const string &info,const T &x,int n = 0,bool clr = true){
	if(clr){
		system("cls");
	}
	cout << endl << "\\**********************" << endl << info << endl;
	ydelon = n;
	if(ydelon >= 0){
		cout << x;
	}
	else{
		return false;
	}
	cout << endl << "**********************\\" << endl;
	return true;
}
//list & map & vector
template<typename T,typename S>
ostream &operator<<(ostream &os,const pair<T,S> &it){
    return 	os << it.first << " " << it.second;
}
template<typename T,typename S>
ostream &operator<<(ostream &os,const map<T,S> &st){
	int n = ydelon==0?st.size():ydelon,i = 0;
	os <<  " size=" << st.size() << " show=" << n << endl;
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
	int n = ydelon==0?st.size():ydelon,i = 0;
	os <<  " size=" << st.size() << " show=" << n << endl;
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
	int n = ydelon==0?st.size():ydelon,i = 0;
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
int findf(vector<yc2i> vep1,int a){
	if(vep1[a].fa == a){
		return a;
	}
	else{
		vep1[a].fa = findf(vep1,vep1[a].fa);
		return vep1[a].fa;
	}
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
	#ifdef YLOFI
	freopen("yin1.txt","r",stdin);
	//freopen("yout.txt","w",stdout);
	#endif
	#ifdef YDELO
	assert(1);
	#endif
	//kruskal-²¢²éŒ¯Ëã·š 
	int gp = 0;
	int ge = 0;
	cin >> gp >> ge;
	list<yc3i> lie1;//±ßÐÅÏ¢
	for(int i = 0;i<ge;i++){
		yc3i bufy1;
		cin >> bufy1.from >> bufy1.to >> bufy1.len;
		bufy1.from--;
		bufy1.to--;
		lie1.push_back(bufy1); 
	}
	lie1.sort();
	vector<yc2i> vep1;//µãÐÅÏ¢(0s) 
	for(int i = 0;i<gp;i++){
		yc2i ybuf1;
		ybuf1.fa = i;
		ybuf1.rank = 0;
		vep1.push_back(ybuf1);
	}
	list<yc3i>::iterator itl1 = lie1.begin();
	int bufsize1 = lie1.size();
	for(int i = 0;i<bufsize1;i++,itl1++){
		int buffrom = (*itl1).from;//×ó¶Ëµã 
		int bufto = (*itl1).to;//ÓÒ¶Ëµã 
		int bufffa = findf(vep1,buffrom);
		int buftfa = findf(vep1,bufto);
		if(bufffa != buftfa){
			if(vep1[bufffa].rank > vep1[buftfa].rank){
				vep1[buftfa].fa = vep1[bufffa].fa;
			}
			else if(vep1[bufffa].rank < vep1[buftfa].rank){
				vep1[bufffa].fa = vep1[buftfa].fa;
			}
			else{
				vep1[buftfa].fa = vep1[bufffa].fa;
				vep1[bufffa].rank++;
			}
			int buffas1 = findf(vep1,0);
			int buffae1 = findf(vep1,gp-1);
			if(buffas1 == buffae1){
				cout << (*itl1).len;
				break;
			}
		}
	}
	return 0;
}
