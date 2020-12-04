//宏观dfs微观bfs
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
struct yc2i{
	int to;//0s
	int len;
};
#ifdef YDELO
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
//自定义
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
bool dfs(vector<vector<yc2i> > &vee1,int len){
	vector<int> vep1;//点信息(0s) 0未遍历
	int gp = vee1.size();
	for(int i = 0;i<gp;i++){
		vep1.push_back(0);
	}
	list<int> lis1;//bfs连通栈(0s) 
	int head = 0;
	lis1.push_front(head);
	while(!lis1.empty()){
		head = lis1.front();
		lis1.pop_front();
		int bufsize1 = vee1[head].size();
		for(int i = 0;i<bufsize1;i++){
			int bufto = vee1[head][i].to;
			int buflen = vee1[head][i].len;
			if(vep1[bufto] == 0 && buflen<=len){
				lis1.push_front(bufto);
				vep1[bufto] = 1;
				if(bufto == gp-1){
					return true;
				}
			}
		}		
	}
	return false;
}
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	#ifdef YLOFI
	freopen("yin3.txt","r",stdin);
	//freopen("yout.txt","w",stdout);
	#endif
	#ifdef YDELO
	assert(1);
	#endif
	//二分法实验各种值 
	//781ms	12.85MB
	int gp = 0;
	int ge = 0;
	cin >> gp >> ge;
	vector<vector<yc2i> > vee1;
	for(int i = 0;i<gp;i++){
		vector<yc2i> bufl1;
		vee1.push_back(bufl1);
	}
	for(int i = 0;i<ge;i++){
		int bufi1 = 0;
		int bufi2 = 0;
		yc2i bufy1;
		cin >> bufi1 >> bufi2 >> bufy1.len;
		bufi1--;
		bufi2--;
		bufy1.to = bufi2;
		vee1[bufi1].push_back(bufy1);
		bufy1.to = bufi1;
		vee1[bufi2].push_back(bufy1);
	}
	#ifdef YDELO
	assert(yPrint("vee1",vee1,0,false));
	#endif
	int gl = 1;
	int gr = 1000000;
	while(gl<gr){
		int m = (gl+gr)/2;
		#ifdef YDELO
		assert(yPrint("m",m,0,false));
		#endif
		if(dfs(vee1,m)){
			gr = m;
		}
		else{
			gl = m+1;
		}
	}
	cout << gl;
	return 0;
}
