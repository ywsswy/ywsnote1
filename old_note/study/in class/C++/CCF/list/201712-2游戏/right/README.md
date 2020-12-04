//#define YLOFI
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
	int gk = 0;
	cin >> gn >> gk;
	vector<int> vev1;//0 die 1s
	vev1.push_back(1);
	for(int i = 0;i<gn;i++){
		vev1.push_back(1);
	}
	int gp = 0;
	gp = gn;
	int i = 0;//考虑到第几个人 
	int num = 0;//报到第几个数 
	while(true){
		if(gp == 1){
			break;
		}
		i++;
		if(i > gn){
			i = 1;
		}
		if(vev1[i] == 0){
			continue;
		}
		num++;
		if(num%gk == 0 || num%10 == gk){
			#ifdef YDELO
			assert(yPrint("num",num,0,false));
			#endif
			vev1[i] = 0;
			gp--;
		}
	}
	for(int i = 0;i<gn;i++){
		int j = i+1;
		if(vev1[j] == 1){
			cout << j;
		}
	}
	return 0;
}
