#include<iostream>
#include<map>
#include<list>
using namespace std;
struct yc2i{
	int value;
	int count;
};
bool operator < (const yc2i &a,const yc2i &b){
	if(a.count < b.count){
		return true;
	}
	else if(a.count == b.count && a.value > b.value){
		return true;
	}
	else{
		return false;
	}
}
int main(){
	freopen("in2.txt","r",stdin);
	int gn = 0;
	cin >> gn;
	map<int,int> ma;
	for(int i = 0;i<gn;i++){
		int buf = 0;
		cin >> buf;
		ma[buf]++;
	}
	list<yc2i> li;
	for(map<int,int>::iterator itm = ma.begin();itm!=ma.end();itm++){
		yc2i buf;
		buf.value = itm->first;
		buf.count = itm->second;
		li.push_back(buf);
	}
	li.sort();
	yc2i res = li.back();
	cout << res.value << endl;
	return 0;
}
