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
#include<cmath> 
using namespace std;
#define YCOL1 10
struct yc2il{
	int v;//遍历标志 0未
	int lay;//层次(0s) 
	list<int> li;//邻接表 
};
#ifdef YDELO
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
int ydelom = 0;
//自定义
ostream &operator<<(ostream &os,const yc2il &st){
	os << "v=" << st.v << " lay=" << st.lay << " li(size=" << st.li.size() << ")=";
	for(list<int>::const_iterator it = st.li.begin();it!=st.li.end();it++){
		os << *it << "=>";
	} 
	return os;
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
	int nr = 0;
	int nt = 0;
	cin >> nr >> nt;
	map<int,yc2il> ma;//设备联通关系图 K:设备号 正路由(1s)负电脑 V:邻接表
	yc2il bufy1;
	bufy1.v = 0;
	ma[1] = bufy1;
	//路由器 
	for(int i = 0;i<nr-1;i++){
		int bufi1 = 0;
		cin >> bufi1;
		ma[bufi1].li.push_back(i+2);
		yc2il bufy2;
		bufy2.v = 0;
		bufy2.li.push_back(bufi1);
		ma[i+2] = bufy2;
	}
	//电脑 
	for(int i = 0;i<nt;i++){
		int bufi1 = 0;
		cin >> bufi1;
		ma[bufi1].li.push_back(-i-1);
		yc2il bufy2;
		bufy2.v = 0;
		bufy2.li.push_back(bufi1);
		ma[-i-1] = bufy2;
	}
	#ifdef YDELO
	assert(yPrint("ma",ma,0,false)); 
	#endif
	//第一次最远点 
	list<int> li;//遍历队列 V:设备号
	li.push_back(1);
	ma[1].v = 1; 
	int head = 0; 
	while(!li.empty()){
		head = li.front();
		li.pop_front();
		for(list<int>::iterator it = ma[head].li.begin();it != ma[head].li.end();it++){
			if(!ma[*it].v){
				li.push_back(*it);
				ma[*it].v = 1;
			}
		}
	}
	#ifdef YDELO
	assert(yPrint("head",head,0,false)); 
	#endif
	//第二次最远点
	//1)清空访问标志
	for(map<int,yc2il>::iterator it = ma.begin();it != ma.end();it++){
		it->second.v = 0;
	}
	#ifdef YDELO
	assert(yPrint("ma 2",ma,0,false)); 
	#endif
	//2)获取最深层次 
	li.push_back(head);
	ma[head].v = 1;
	ma[head].lay = 0; 
	while(!li.empty()){
		head = li.front();
		li.pop_front();
		for(list<int>::iterator it = ma[head].li.begin();it!=ma[head].li.end();it++){
			if(!ma[*it].v){
				li.push_back(*it);
				ma[*it].v = 1;
				ma[*it].lay = ma[head].lay + 1;
			}
		}
	}
	#ifdef YDELO
	assert(yPrint("ma 3",ma,0,false)); 
	#endif
	cout << ma[head].lay;
	return 0;
}
