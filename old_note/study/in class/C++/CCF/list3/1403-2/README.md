#define YLOFI
#define YDELO
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
#ifdef YDELO
int ydelon = 0;
//for built-in type(except c_array) & map & list & vector
template<typename T>
void yPrint(const string &info,const T &x,int n = 0,bool clr = true){
	ydelon = n;
	if(clr){
		system("clear");//system("cls"); in windows
	}
	cout << endl << "\\**********************" << endl;
	cout << info << endl;
	cout << x << "**********************\\" << endl;
	return;
}
template<typename T,typename S>
ostream &operator<<(ostream &os,const pair<T,S> &it){
    return 	os << it.first << " " << it.second;
}
template<typename T,typename S>
ostream &operator<<(ostream &os,const map<T,S> &st){
	int n = (ydelon==0?st.size():ydelon);
	os << " size=" << st.size() << " show=" << n << endl;
	typename map<T,S>::const_iterator it = st.begin();
	for(int i = 0;i<n && i<st.size();i++,it++){
		os << i << " " << *it << "*" << endl;
	}
	return os;
}
template<typename T>
ostream &operator<<(ostream &os,const list<T> &st){
	int n = (ydelon==0?st.size():ydelon);
	os << " size=" << st.size() << " show=" << n << endl;
	typename list<T>::const_iterator it = st.begin();
	for(int i = 0;i<n && i<st.size();i++,it++){
		os << i << " " << *it << "*" << endl;
	}
	return os;
}
template<typename T>
ostream &operator<<(ostream &os,const vector<T> &st){
	int n = (ydelon==0?st.size():ydelon);
	os << " size=" << st.size() << " show=" << n << endl;
	typename vector<T>::const_iterator it = st.begin();
	for(int i = 0;i<n && i<st.size();i++,it++){
		os << i << " " << *it << "*" << endl;
	}
	return os;
}
#endif
struct yc2i{
	int x;
	int y;
};
struct ycw1{
	yc2i le;
	yc2i ri;
	int num;
};
ostream &operator<<(ostream &os,const yc2i &yc){
	os << "x=" << yc.x << ",y=" << yc.y;
	return os;
}
ostream &operator<<(ostream &os,const ycw1 &yc){
	os << "le=" << yc.le << ",ri=" << yc.ri;
	return os;
}
bool operator<(const yc2i &yc1,const yc2i &yc2){
	return yc1.x <= yc2.x && yc1.y <= yc2.y;
}
bool operator>(const yc2i &yc1,const yc2i &yc2){
	return yc1.x >= yc2.x && yc1.y >= yc2.y;
}
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	#ifdef YLOFI
	freopen("test.txt","r",stdin);
	//freopen("yout1.txt","w",stdout);
	#endif
	#ifdef YDELO
	#endif
	int gn = 0;
	int gm = 0;
	cin >> gn;
	cin >> gm;
	list<ycw1> gliw1;
	for(int i = 0;i<gn;i++){
		ycw1 buf1;
		cin >> buf1.le.x >> buf1.le.y >> buf1.ri.x >> buf1.ri.y;
		buf1.num = i+1;
		gliw1.push_front(buf1);
	}
	for(int i = 0;i<gm;i++){
		yc2i buf1;
		cin >> buf1.x >> buf1.y;
		list<ycw1>::iterator itl = gliw1.begin();
		int size = gliw1.size();
		int j = 0;
		for(;j<size;j++,itl++){
			if(buf1 > itl->le && buf1 < itl->ri){
				cout<< itl->num << endl;
				gliw1.push_front(*itl);
				gliw1.erase(itl);
				break;
			}
		}
		if(j == size){
			cout << "IGNORED" << endl;
		}
	}
	return 0;
}
