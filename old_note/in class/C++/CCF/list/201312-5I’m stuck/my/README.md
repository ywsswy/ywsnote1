#include<iostream>
#include<cstdio>
#include<string>
#include<vector>
#include<map>
#include<deque>
#include<list>
#include<cstring>
#include<stack>
using namespace std;
bool isleft(int i,int j,vector<vector<char> > &f);
bool isright(int i,int j,vector<vector<char> > &f);
bool isup(int i,int j,vector<vector<char> > &f);
bool isdown(int i,int j,vector<vector<char> > &f);
void dfs(vector<vector<char> > &f,stack<pair<int,int> > &s,vector<char> &sf,int i,int j);
void dfs2(vector<vector<char> > &f,stack<pair<int,int> > &e,vector<char> &sf,int i,int j);
int r,c;
int main(){
	// 0 1 2 3 up down left right
	freopen("in2.txt","r",stdin);
	cin >> r;
	cin >> c;
	int si,sj;
	int ei,ej;	
	vector<vector<char> > f;
	vector<char> fb;
	for(int i = 0;i<r;i++){
		fb.clear();
		for(int j = 0;j<c;j++){
			char buf;
			cin>>buf;
			if(buf != '+' 
				&& 
				buf != '-' 
				&&
				buf != '|' 
				&& 
				buf != '.'
				&& 
				buf != '#' 
				&& 
				buf != 'S'
				&&
				buf != 'T'){
					continue;
			}
			if(buf == 'S'){
				si = i;
				sj = j;
			}
			if(buf == 'T'){
				ei = i;
				ej = j;
			}
			fb.push_back(buf);
		}
		f.push_back(fb);
	}
	vector<char> sf;//1 : 可以从起点到达的点
	for(int i = 0;i<r*c;i++){
		sf.push_back(0);
	}
	stack<pair<int,int> > s;
	dfs(f,s,sf,si,sj);
	
	vector<char> ef;//1 : 可以到达终点的点
	for(int i = 0;i<r*c;i++){
		ef.push_back(0);
	}
	stack<pair<int,int> > e;
	dfs2(f,e,ef,ei,ej);
	int suc = 0;
	for(int i = 0;i<r;i++){
		for(int j = 0;j<c;j++){
			if(sf[c*i+j] == 1 && ef[c*i+j] == 0){
				suc++;
			}
		}
	}
	if(sf[c*ei+ej] == 0){
		cout<<"I'm stuck!";
	}
	else{
		cout<<suc;
	}
	return 0;
}
void dfs2(vector<vector<char> > &f,stack<pair<int,int> > &e,vector<char> &ef,int i,int j){
	pair<int,int> m = make_pair(i,j);
	e.push(m);
	ef[i*c+j] = 1;
	if(isdown(i-1,j,f)){
		if(ef[c*(i-1)+j] == 0){
			dfs2(f,e,ef,i-1,j);
		}
	}
	if(isup(i+1,j,f)){
		if(ef[c*(i+1)+j] == 0){
			dfs2(f,e,ef,i+1,j);
		}
	}
	if(isright(i,j-1,f)){
		if(ef[c*i+j-1] == 0){
			dfs2(f,e,ef,i,j-1);
		}
	}
	if(isleft(i,j+1,f)){
		if(ef[c*i+j+1] == 0){
			dfs2(f,e,ef,i,j+1);
		}
	}
	e.pop();
}
void dfs(vector<vector<char> > &f,stack<pair<int,int> > &s,vector<char> &sf,int i,int j){
	pair<int,int> m = make_pair(i,j);
	s.push(m);
	sf[i*c+j] = 1;
	if(isup(i,j,f)){
		if(sf[c*(i-1)+j] == 0){
			dfs(f,s,sf,i-1,j);
		}
	}
	if(isdown(i,j,f)){
		if(sf[c*(i+1)+j] == 0){
			dfs(f,s,sf,i+1,j);
		}
	}
	if(isleft(i,j,f)){
		if(sf[c*i+j-1] == 0){
			dfs(f,s,sf,i,j-1);
		}
	}
	if(isright(i,j,f)){
		if(sf[c*i+j+1] == 0){
			dfs(f,s,sf,i,j+1);
		}
	}
	s.pop();
}
bool isup(int i,int j,vector<vector<char> > &f){
	if(i <= 0
			||
		i >= r
			||
		j < 0
			||
		j >= c
			||
		f[i][j] == '-'
			||
		f[i][j] == '.'
			||
		f[i][j] == '#'){
		return false;
	}
	if(f[i-1][j] == '#'){
		return false;
	}
	return true;
}
bool isdown(int i,int j,vector<vector<char> > &f){
	if(i >= r-1
			||
		i < 0
			||
		j < 0
			||
		j >= c
			||
		f[i][j] == '-'
			||
		f[i][j] == '#'){
			return false;
	}
	if(f[i+1][j] == '#'){
		return false;
	}
	return true;
}
bool isleft(int i,int j,vector<vector<char> > &f){
	if(j <= 0
			||
		j >= c
			||
		i < 0
			||
		i >= r
			||
		f[i][j] == '|'
			||
		f[i][j] == '.'
			||
		f[i][j] == '#'){
			return false;
	}
	if(f[i][j-1] == '#'){
		return false;
	}
	return true;
}
bool isright(int i,int j,vector<vector<char> > &f){
	if(j >= c-1
			||
		j < 0
			||
		i < 0
			||
		i >= r
			||
		f[i][j] == '|'
			||
		f[i][j] == '.'
			||
		f[i][j] == '#'){
			return false;
	}
	if(f[i][j+1] == '#'){
		return false;
	}
	return true;
}

