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
#define YCOL1 10
class yc3i{
public:
	int time;//遍历次数(0s)
	int vi;//是否在队列中 0：不在 
	int len;//此时算出的该点的邻接的瓶颈路端
};
#ifdef YDELO
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
int ydelom = 0;
//自定义
std::ostream &operator<<(std::ostream &os,const yc3i &st){
	os << "time=" << st.time << ",vi=" << st.vi << ",len=" << st.len;
	return os;
}
bool operator<(const yc3i &yc1,const yc3i &yc2){
	return yc1.i1 < yc2.i2;
}
//二维数组
template<typename T>
void yPrintArr(const T x[][YCOL1]){
	int i = 0;
	while(1){
		cout << i;
		for(int j = 0;j<ydelom;j++){
			cout << " (" << j << "," << x[i][j] << ")";
		}
		i++;
		if(i >= ydelon){
			break;
		}
		else{
			cout << endl;
		}
	}
	return;
}
template<typename T>
bool yPrint(const string &info,const T x[][YCOL1],int n = 0,int m = 0,bool clr = true){
	if(clr){
		system("cls");
	}
	cout << endl << "\\**********************" << endl << info << endl;
	ydelon = n;
	ydelom = m;
	if(ydelon > 0 && ydelom > 0){
		yPrintArr(x);
	}
	else{
		return false;
	}
	cout << endl << "**********************\\" << endl;
	return true;
}
//数组
template<typename T,int size>
void yPrintArr(const T (&x)[size]){
	int i = 0;
	while(1){
		cout << i << " " << x[i];
		i++;
		if(i >= ydelon){
			break;
		}
		else{
			cout << endl;
		}
	}
	return;
}
template<typename T,int size>
bool yPrint(const string &info,const T (&x)[size],int n = 0,bool clr = true){
	if(clr){
		system("cls");
	}
	cout << endl << "\\**********************" << endl << info << endl;
	ydelon = n;
	if(ydelon > 0){
		yPrintArr(x);
	}
	else{
		return false;
	}
	cout << endl << "**********************\\" << endl;
	return true;
}
//非数组
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
	return 0;
}
