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
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	#ifdef YLOFI
	freopen("yin1.txt","r",stdin);
	#endif
	#ifdef YDELO
	assert(1);
	#endif
	int gn = 0;
	cin >> gn;
	list<int> li1;
	for(int i = 0;i<gn;i++){
		int bufi = 0;
		cin >> bufi;
		li1.push_back(bufi);
	}
	#ifdef YDELO
	assert(yPrint("li1",li1,0,false));
	#endif
	li1.sort();
	#ifdef YDELO
	assert(yPrint("li1",li1,0,false));
	#endif
	int rmin = 99999;
	int now = 0;
	int pre = 0;
	int yabs = 0;
	list<int>::iterator it = li1.begin();
	pre = *it;
	for(int i = 0;i<gn-1;i++){
		it++;
		now = *it;
		if(now-pre < 0){
			yabs = pre-now;
		}
		else{
			yabs = now-pre;
		}
		rmin = min(rmin,yabs);
		pre = now;
	}
	cout << rmin;	
	return 0;
}
