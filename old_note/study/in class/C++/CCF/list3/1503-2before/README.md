#include<iostream>
#include<map>
#include<set>
using namespace std;
int main(){
	freopen("in2.txt","r",stdin);
	int gn;
	map<int,int> m;
	cin >> gn;
	for(int i = 0;i<gn;i++){
		int buf;
		cin >> buf;
		if(m.find(buf) != m.end()){
			m[buf]++;
		}
		else{
			m[buf] = 1;
		}
	}
	set<int> s;
	for(map<int,int>::iterator it = m.begin();it!=m.end();it++){
		if(s.find(it->second) == s.end()){
			s.insert(it->second);
		}
	}
	while(!s.empty()){
		set<int>::iterator its = s.end();
		its--;
		for(map<int,int>::iterator it = m.begin();it!=m.end();it++){
			if(it->second == *its){
				cout << it->first << ' ' << it->second << endl;
			}
		}
		s.erase(its);
	}
	return 0;
}
