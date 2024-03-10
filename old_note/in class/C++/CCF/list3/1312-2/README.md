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
        string gs1 = "";
        cin >> gs1;
        //yCheck
        gs1.replace(1,1,"");
        gs1.replace(4,1,"");
        int gcheck = 0;
        for(int i = 0;i<9;i++){
            gcheck +=((i+1)*(gs1[i]-'0'));
        }
        int gmod = gcheck%11;
        char gshould = (gmod==10)?'X':('0'+gmod);
        if(gs1[10] == gshould){
            cout << "Right" << endl;
        }
        else{
            cout <<gs1[0]<<'-'<<
                    gs1.substr(1,3)<<'-'<<
                    gs1.substr(4,5)<<'-'<<
                    gshould<<endl;                    
        }
	return 0;
}
