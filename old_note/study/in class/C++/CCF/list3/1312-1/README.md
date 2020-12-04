/*
这题map 和 vector 都能做
*/
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
template<typename T>
void yPrint(const string &info,const T &x,int n = 0,bool clr = true){
	ydelon = n;
	if(clr){
		system("clear");
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
int main(){
	ios::sync_with_stdio(false);
	cin.tie(0);
	#ifdef YLOFI
	freopen("yin1.txt","r",stdin);
	//freopen("yout1.txt","w",stdout);
	#endif
	#ifdef YDELO
	#endif
        int gn = 0;
        cin >> gn;
        map<int,int> ma1;
        for(int i = 0;i<gn;i++){
            int buf = 0;
            cin >> buf;
            ma1[buf]++;
        }
        int gmaxkey1 = 0;
        {
            map<int,int>::iterator itm1 = ma1.begin();
            gmaxkey1 = itm1->first;
            int size1 = ma1.size();
            int maxval1 = itm1->second;
            for(int i = 0;i<size1;i++,itm1++){
                if(itm1->second>maxval1){
                    gmaxkey1 = itm1->first;
                    maxval1 = itm1->second;
                }
            }
        }
        cout << gmaxkey1 << endl;
	return 0;
}
