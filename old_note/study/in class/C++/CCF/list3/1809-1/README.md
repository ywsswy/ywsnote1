//#define YWATCH
//#define YLFILE
#include<iostream>
#include<iomanip>
#include<string>
#include<sstream>
#include<fstream>
#include<vector>
#include<list>
#include<map>
#include<algorithm>
#include<cmath>
using namespace std;
#ifdef YWATCH
#define YGETSIZE size_t n = (ydelon == 0 ? st.size() : ydelon); os << " size=" << st.size() << " show=" << n << "\n";typename
#define YOUTPUT it = st.begin(); for (size_t i = 0; i < n && i < st.size(); i++, it++) { os << i << " " << *it << "*\n"; } return os;
int ydelon = 0;
/*for built-in type(except c_array) & map & list & vector*/
template<typename T>
void yPrint(const string &info, const T &x, int n = 0, bool clr = true) {
    ydelon = n;
    if (clr) system("clear"); //system("cls"); in windows
    cout << "\n\\**********************\n" << info << "\n" << x << "**********************\\\n";
    return;
}
template<typename T, typename S>
ostream &operator<<(ostream &os, const pair<T, S> &st) { return os << st.first << " " << st.second; }
template<typename T, typename S>
ostream &operator<<(ostream &os, const map<T, S> &st) { YGETSIZE map<T, S>::const_iterator YOUTPUT }
template<typename T>
ostream &operator<<(ostream &os, const list<T> &st) { YGETSIZE list<T>::const_iterator YOUTPUT; }
template<typename T>
ostream &operator<<(ostream &os, const vector<T> &st) { YGETSIZE vector<T>::const_iterator YOUTPUT; }
#endif
int main() {
    cin.tie(0);
#ifdef YLFILE
    ifstream filei("yin1.txt");
    ofstream fileo("yout1.txt");
    streambuf *backi = cin.rdbuf();
    streambuf *backo = cout.rdbuf();
    cin.rdbuf(filei.rdbuf());
    //cout.rdbuf(fileo.rdbuf());
#endif
    int gn = 0;
    cin >> gn;
    vector<int> ve;//1s
    ve.resize(gn+2,0);
    for(int i = 0+1;i<gn+1;i++){
        cin >> ve[i];
    }
    cout << (ve[1] + ve[2])/2 << " ";
    for(int i = 0+1;i<gn-2+1;i++){
        cout << (ve[i]+ve[i+1]+ve[i+2])/3 << " ";
    }
    cout << (ve[gn-1] + ve[gn])/2 << " ";
#ifdef YLFILE
    cin.rdbuf(backi);
    //cout.rdbuf(backo);
#endif
    return 0;
}

