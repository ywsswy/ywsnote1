#include<iostream>
#include<cstdio>
#include<string>
#include<vector>
#include<deque>
#include<cstring>
#include<list>
#include<map>
#include<stack>
#include<cmath>
using namespace std;
struct marr{
	int val[4];
};
int main(){
	freopen("in2.txt","r",stdin);
	int nw;
	int nc;
	cin >> nw >> nc;
	list<int> l;
	vector<struct marr> v;
	for(int i = 0;i<nw;i++){
		struct marr buf;
		l.push_front(i);
		cin >> buf.val[0];
		cin >> buf.val[1];
		cin >> buf.val[2];
		cin >> buf.val[3];
		v.push_back(buf);
	}
	for(int i = 0;i<nc;i++){
		int bx;
		int by;
		cin >> bx;
		cin >> by;
		int flag = 0;
		int newlay;
		for(list<int>::iterator it = l.begin();it != l.end();it++){
			int lay = *it;
			if(bx>=v[lay].val[0] && bx<=v[lay].val[2] && by>=v[lay].val[1] && by<=v[lay].val[3]){
				newlay = lay;
				l.erase(it);
				l.push_front(lay);
				flag = 1;
				break;
			}
		}
		if(flag == 0){
			cout << "IGNORED" <<endl;
		}
		else{
			cout << newlay+1 <<endl;
		}
	}
	return 0;
}
