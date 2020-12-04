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
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
#ifdef YLOFI
    freopen("yin1.txt", "r", stdin);
    //freopen("yout1.txt","w",stdout);
#endif
#ifdef YDELO
#endif
    int gr = 0;
    int gc = 0;
    cin >> gr >> gc;
    vector<vector<int> > gve1;
    {
        vector<int> bufv;
        bufv.resize(gc,-1);
        gve1.resize(gr,bufv);
    }
    for(int i = 0;i<gr;i++){
        for(int j = 0;j<gc;j++){
            cin >> gve1[i][j];
        }
    }
    for(int i = 0;i<gc;i++){
        for(int j = 0;j<gr;j++){
            cout << gve1[j][gc-i-1] << " ";
        }
        cout << endl;
    }
    return 0;
}
