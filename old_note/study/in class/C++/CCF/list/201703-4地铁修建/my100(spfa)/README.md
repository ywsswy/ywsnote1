#define YLOFI
#define YDELO
#include<iostream>
#include<iomanip>
#include<cstdio>
#include<string>
#include<sstream>
#include<map>
#include<list>
#include<vector>
#include<algorithm>
#include<cmath> 
using namespace std;
struct yc3i{
	int time;//遍历次数(0s)
	int vi;//是否在队列中 0：不在 
	int len;//此时算出的该点的邻接的瓶颈路端
};
struct yc2i{
	int to;
	int len;
};
#ifdef YDELO
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
//自定义
ostream &operator<<(ostream &os,const yc3i &st){
	os << "time=" << st.time << ",vi=" << st.vi << ",len=" << st.len;
	return os;
}
ostream &operator<<(ostream &os,const yc2i &st){
	os << "to=" << st.to << ",len=" << st.len;
	return os;
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
	#ifdef YLOFI
	freopen("yin.txt","r",stdin);
	//freopen("yout.txt","w",stdout);
	#endif
	#ifdef YDELO
	assert(1);
	#endif
	//SPFA 单源最短路径算法 
	//687ms	14.28MB
	int gp = 0;//点数 
	int ge = 0;//边数 
	cin >> gp >> ge;
	vector<vector<yc2i> >ve1;//边信息:每点(0s)的临接边 
	for(int i = 0;i<gp;i++){
		vector<yc2i> ve1buf;
		ve1.push_back(ve1buf);
	}
	for(int i = 0;i<ge;i++){
		int bufi1 = 0;
		int bufi2 = 0;
		yc2i bufy1;
		//cin >> bufi1 >> bufi2 >> bufy1.len;
		scanf("%d%d%d",&bufi1,&bufi2,&bufy1.len);
		bufi1--;
		bufi2--; 
		bufy1.to = bufi2;
		ve1[bufi1].push_back(bufy1);
		bufy1.to = bufi1;
		ve1[bufi2].push_back(bufy1);
	}
	vector<yc3i> ve2;//点信息 K:点(0s) V:点信息
	//初始化点信息
	const int inf = 0x7fffffff;
	for(int i = 0;i<gp;i++){
		yc3i bufy1;
		bufy1.vi = 0;
		bufy1.len = inf;
		bufy1.time = 0;
		ve2.push_back(bufy1);
	}
	#ifdef YDELO
	assert(yPrint("ve2",ve2,0,false));
	#endif
	list<int> li;//待优化队列
	int head = 0;
	ve2[head].len = 0;
	ve2[head].vi = 1;
	ve2[head].time++;
	li.push_back(head);
	int flagno = 0;//1:存在负环，无法使用SPFA 
	while(!li.empty() && flagno == 0){
		head = li.front();//围绕这个点进行松弛 
		li.pop_front();
		ve2[head].vi= 0;
		for(int i = 0;i<ve1[head].size();i++){ 
			int bufto = ve1[head][i].to;//该条边另一个端点 
			int buflen = ve1[head][i].len;//该条边长度 
			//可以进行松弛 
			int bufmax = max(ve2[head].len,buflen);
			if(ve2[bufto].len > bufmax){
				ve2[bufto].len = bufmax;
				ve2[bufto].time++;
				if(ve2[bufto].time >= gp){//!!!
					flagno = 1;
					break;
				}
				//松弛的更新点入队 
				if(ve2[bufto].vi == 0){
					ve2[bufto].vi = 1;
					//STL优化 
					if(!li.empty() && ve2[bufto].len <= ve2[li.front()].len){
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
	assert(yPrint("ve2",ve2,0,false));
	#endif
	//存在负环情况 
	if(flagno == 1){
	}
	else{
		cout << ve2[gp-1].len;
	}
	return 0;
}
