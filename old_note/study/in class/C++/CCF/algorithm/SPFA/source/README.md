#define YLOFI
#define YDELO
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
struct yc4i{
	int time;//遍历次数(0s)
	int vi;//是否在队列中 0：不在 
	int len;//此时算出的从单源点到此的最短路径距离 
	int from;//此时假设更新为最短路径，该点是由哪个点走过来的（用于保存逆向路径） 
};
#ifdef YDELO
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
int ydelom = 0;
//自定义
ostream &operator<<(ostream &os,const yc4i &st){
	os << "time=" << st.time << ",vi=" << st.vi << ",len=" << st.len << ",from=" << st.from; 
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
	//SPFA 单源最短路径算法 
	int gp = 0;//点数 
	int ge = 0;//边数 
	cin >> gp >> ge;
	map<int,map<int,int> > ma1;//边信息 K:from(1s) V:(K:to V:len) 
	for(int i = 0;i<ge;i++){
		int bufi1 = 0;
		int bufi2 = 0;
		int bufi3 = 0;
		cin >> bufi1 >> bufi2 >> bufi3;
		ma1[bufi1][bufi2] = bufi3;
		ma1[bufi2][bufi1] = bufi3;
	}
	map<int,yc4i> ma2;//点信息 K:点(1s) V:点信息
	//初始化点信息
	const int inf = 0x3fffffff;//!!!
	for(int i = 0;i<gp;i++){
		ma2[i+1].vi = 0;
		ma2[i+1].len = inf;
		ma2[i+1].time = 0;
		ma2[i+1].from = 0;
	}
	list<int> li;//待优化队列
	int head = 1;
	ma2[head].len = 0;
	ma2[head].vi = 1;
	ma2[head].time++;
	li.push_back(head);
	int flagno = 0;//1:存在负环，无法使用SPFA 
	while(!li.empty() && flagno == 0){
		head = li.front();//围绕这个点进行松弛 
		li.pop_front();
		ma2[head].vi= 0;
		for(map<int,int>::iterator itm = ma1[head].begin();itm!=ma1[head].end();itm++){
			int bufto = itm->first;//该条边另一个端点 
			int buflen = itm->second;//该条边长度 
			//可以进行松弛 
			if(ma2[bufto].len > ma2[head].len + buflen){
				ma2[bufto].len = ma2[head].len + buflen;
				ma2[bufto].from = head;
				ma2[bufto].time++;
				if(ma2[bufto].time >= gp){//!!!
					flagno = 1;
					break;
				}
				//松弛的更新点入队 
				if(ma2[bufto].vi == 0){
					ma2[bufto].vi = 1;
					//STL优化 
					if(!li.empty() && ma2[bufto].len <= ma2[li.front()].len){
						li.push_front(bufto);
					}
					else{
						li.push_back(bufto);
					}
				}
			}
		}
	}
	#ifdef YDELO
	assert(yPrint("ma2",ma2,0,false));
	#endif
	//存在负环情况 
	if(flagno == 1){
	}
	else{
		//逆向输出路径 
		int bufi1 = 0;
		int bufi2 = 0;
		int maxsublen = 0; 
		
		bufi1 = gp;
		while(bufi1!=0){
			#ifdef YDELO
			cout << bufi1 << "<=";
			#endif
			bufi2 = ma2[bufi1].from;
			maxsublen = max(maxsublen,ma1[bufi1][bufi2]);
			bufi1 = bufi2;			
		} 
		cout << maxsublen;
	}
	
	return 0;
}
