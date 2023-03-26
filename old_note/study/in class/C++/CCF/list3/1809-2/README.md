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
void yCrossMerge(map<int, int> &ma, const int h, const int t) {
    int head = min(h, t);
    int tail = max(h, t);
    map<int, int>::iterator itm = ma.begin();
    int size = ma.size();
    int i = 0;
    for (; i < size; i++, itm++) {
        if (itm->second + 1 < h) {
            continue;
        } else {
            head = min(head, itm->first);
            break;
        }
    }
    while (i < size) {
        if (itm->first - 1 > tail) {
            break;
        } else {
            tail = max(tail, itm->second);
            itm++;
            map<int, int>::iterator itbuf = itm;
            itm--;
            ma.erase(itm);
            itm = itbuf;
            i++;
        }
    }
    ma[head] = tail;
    return;
}
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
    int n = 0;
    cin >> n;
    map<int,int> ma;
    int hwt = 0;
    for(int i = 0;i<n;i++){
        int a = 0;
        int b = 0;
        cin >> a >> b;
        hwt += (b-a);
        if(a != b){
            ma[a] = b-1;
        }
    }
    for(int i = 0;i<n;i++){
        int a = 0;
        int b = 0;
        cin >> a >> b;
        hwt += (b-a);
        if(a != b){
            yCrossMerge(ma,a,b-1);
        }
    }
    int ct = 0;
    size_t msize = ma.size();
    map<int,int>::iterator it = ma.begin();
    for(int i = 0;i<msize;i++,it++){
        ct += (it->second - it->first + 1);
    }
    cout << hwt - ct << "\n";
#ifdef YLFILE
    cin.rdbuf(backi);
    //cout.rdbuf(backo);
#endif
    return 0;
}

