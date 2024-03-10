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
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
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
	int gn = 0;
	cin >> gn;
	map<int,int> ma1;
	for(int i = 0;i<gn;i++){
		int buf = 0;
		cin >> buf;
		ma1[buf]++;
	}
	int sum1 = 0;
	int size1 = ma1.size();
	map<int,int>::iterator itm1 = ma1.begin();
	for(int i = 0;i<size1;i++){
		int bufs1 = itm1->second;
		int bufs2 = sum1*2;
		if(bufs2+bufs1 == gn){
			cout << itm1->first;
			return 0;
		}
		else{
			sum1 += bufs1;
			itm1++;
			if(bufs2 >= gn){
				break;
			}
		}
	}
	cout << "-1";	
	return 0;
}
