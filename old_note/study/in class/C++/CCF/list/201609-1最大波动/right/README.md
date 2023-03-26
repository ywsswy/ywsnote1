#define YLOFI
//#define YDELO
#include<iostream>
#include<iomanip>
#include<cstdio>
#include<string>
#include<sstream>
#include<map>
#include<list>
#include<algorithm>
using namespace std;
#ifdef YDELO
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
int ydelom = 0;
//二维数组
/*
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
*/
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
//list & map
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
#endif
int main(){
	#ifdef YLOFI
	freopen("yin.txt","r",stdin);
	//freopen("yout.txt","w",stdout);
	#endif
	#ifdef YDELO
	assert(1);
	#endif
	int n = 0;
	cin >> n;
	int arr[1004] = {0};
	for(int i = 0;i<n;i++){
		cin >> arr[i];
		if(arr[i]<0){
			arr[i] = -arr[i];
		}
	}
	int maxch = 0;
	for(int i = 0;i<n-1;i++){
		int buf = arr[i]-arr[i+1];
		if(buf<0){
			buf = -buf;
		}
		maxch = max(buf,maxch);	
		#ifdef YDELO
		assert(yPrint("buf",buf,0,false));
		assert(yPrint("maxch",maxch,0,false));
		#endif
	}
	cout << maxch;		
	return 0;
}
