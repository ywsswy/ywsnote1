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
/*for built-in type(except c_array) & map & list & vector*/
template<typename T>
void yPrint(const string &info, const T &x, int n = 0, bool clr = true) {
    ydelon = n;
    if (clr) {
        system("clear"); //system("cls"); in windows
    }
    cout << endl << "\\**********************" << endl;
    cout << info << endl;
    cout << x << "**********************\\" << endl;
    return;
}
template<typename T, typename S>
ostream &operator<<(ostream &os, const pair<T, S> &it) {
    return os << it.first << " " << it.second;
}
template<typename T, typename S>
ostream &operator<<(ostream &os, const map<T, S> &st) {
    int n = (ydelon == 0 ? st.size() : ydelon);
    os << " size=" << st.size() << " show=" << n << endl;
    typename map<T, S>::const_iterator it = st.begin();
    for (int i = 0; i < n && i < st.size(); i++, it++) {
        os << i << " " << *it << "*" << endl;
    }
    return os;
}
template<typename T>
ostream &operator<<(ostream &os, const list<T> &st) {
    int n = (ydelon == 0 ? st.size() : ydelon);
    os << " size=" << st.size() << " show=" << n << endl;
    typename list<T>::const_iterator it = st.begin();
    for (int i = 0; i < n && i < st.size(); i++, it++) {
        os << i << " " << *it << "*" << endl;
    }
    return os;
}
template<typename T>
ostream &operator<<(ostream &os, const vector<T> &st) {
    int n = (ydelon == 0 ? st.size() : ydelon);
    os << " size=" << st.size() << " show=" << n << endl;
    typename vector<T>::const_iterator it = st.begin();
    for (int i = 0; i < n && i < st.size(); i++, it++) {
        os << i << " " << *it << "*" << endl;
    }
    return os;
}
#endif
void yToUpper(string &s){
	int len = s.length();
	for(int i = 0;i<len;i++){
		s[i] = toupper(s[i]);//tolower
	}
	return;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
#ifdef YLOFI
    freopen("yin1.txt", "r", stdin);
    //freopen("yout1.txt","w",stdout);
#endif
#ifdef YDELO
#endif
    string gmode = "";
    cin >> gmode;
    int gcase = 0;
    int gn = 0;
    cin >> gcase >> gn;
    if(gcase == 0){
        yToUpper(gmode);
    }
    vector<string> gve1;
    vector<string> gve2;
    gve1.resize(gn,"");
    gve2.resize(gn,"");
    for(int i = 0;i<gn;i++){
        cin >> gve1[i];
        gve2[i] = gve1[i];
        if(gcase == 0){
            yToUpper(gve1[i]);
        }
    }
    for(int i = 0;i<gn;i++){
        if(gve1[i].find(gmode,0) != string::npos){
            cout << gve2[i] << endl;
        }
    }
    return 0;
}
