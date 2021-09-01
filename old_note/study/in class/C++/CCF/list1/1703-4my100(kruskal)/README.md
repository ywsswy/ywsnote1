```
//#define YLOFI
//#define YDELO
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
	int from;//0s
	int to;//0s
	int len;
};
bool operator<(const yc3i &x,const yc3i &y){
	return x.len <= y.len;
}
#ifdef YDELO
//#include "YLog.h"
#include "assert.h"
int ydelon = 0;
//自定义
ostream &operator<<(ostream &os,const yc3i &st){
	os << "from=" << st.from << ",to=" << st.to << ",len=" << st.len;
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
	//kruskal 连通域（比没剪枝的并查集快）
	//578ms	13.33MB
	int gp = 0;
	int ge = 0;
	cin >> gp >> ge;
	list<yc3i> li;//边信息
	for(int i = 0;i<ge;i++){
		yc3i bufy1;
		//cin >> bufy1.from >> bufy1.to >> bufy1.len;
		scanf("%d%d%d",&bufy1.from,&bufy1.to,&bufy1.len);
		bufy1.from--;
		bufy1.to--;
		li.push_back(bufy1); 
	}
	li.sort();
	#ifdef YDELO
	assert(yPrint("li",li,0,false));
	#endif	
	vector<vector<int> > ve1;//[]:连通域信息连通域号(0s)，该域内的点集 
	int maxdom = -1;//最大连通域号（已占的） 
	vector<int> ve2;//点信息 []:点号(0s) V:连通域号
	for(int i = 0;i<gp;i++){
		ve2.push_back(maxdom);
	}
	#ifdef YDELO
	assert(yPrint("ve2 点信息",ve2,0,false));
	#endif
	int bufsize1 = li.size();
	list<yc3i>::iterator itl = li.begin();
	#ifdef YDELO
	assert(yPrint("边条数",bufsize1,0,false));
	#endif
	for(int i = 0;i<bufsize1;itl++,i++){
		int bufitfrom = (*itl).from;//路径左端点 
		int bufitto = (*itl).to;//路径右端点
		int bufitfromno = ve2[bufitfrom];//左端点所在连通域号
		int bufittono = ve2[bufitto];//右端点所在连通域号 
		//边的两端点在不同连通域内
		if(bufitfromno == -1 || bufittono == -1 || bufitfromno != bufittono){
			//左端点已有连通域
			if(bufitfromno != -1){
				//右端点没有连通域
				if(bufittono == -1){
					//直接加入左端点的连通域 
					ve2[bufitto] = bufitfromno;
					ve1[bufitfromno].push_back(bufitto);
				}
				else{
					//否则合并两个连通域，点少的加入到点多的连通域中 
					int bufsizeto1 = ve1[bufittono].size();
					int bufsizefrom1 = ve1[bufitfromno].size();
					if(bufsizeto1 > bufsizefrom1){
						for(int j = 0;j<bufsizefrom1;j++){	
							int bufele1 = ve1[bufitfromno][j];
							ve2[bufele1] = bufittono;
							ve1[bufittono].push_back(bufele1);
						}
					}
					else{
						for(int j = 0;j<bufsizeto1;j++){	
							int bufele1 = ve1[bufittono][j];
							ve2[bufele1] = bufitfromno;
							ve1[bufitfromno].push_back(bufele1);
						}
					}
				}
			}
			else if(bufittono != -1){
				//左端点没有连通域，右端点有连通域
				//直接加入右端点的连通域 
				ve2[bufitfrom] = bufittono;
				ve1[bufittono].push_back(bufitfrom);
			}
			else{
				//都没有连通域
				maxdom++;
				vector<int> bufv1;
				ve1.push_back(bufv1);
				ve2[bufitfrom] = maxdom;
				ve2[bufitto] = maxdom;
				ve1[maxdom].push_back(bufitfrom);
				ve1[maxdom].push_back(bufitto);
			}
			//如果连通
			if(ve2[0] == ve2[gp-1] && ve2[0] != -1){
				cout << itl->len;
				break;
			}
		}
	}
	return 0;
}
```