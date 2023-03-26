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
    //freopen("yin1.txt", "r", stdin);
    //freopen("yout1.txt","w",stdout);
#endif
#ifdef YDELO
#endif
    /*
     state:0~5
     * 0:2  to  015
     * 1:20 to  1123
     * 2:201to  224
     * 3:203to  334
     * 4:2013to 44
     * 5:23 to  35
     */
    long long gmod = 1000000007;
    int gn = 0;
    cin >> gn;
    vector<vector<long long> > gve1;
    {
        vector<long long> bufv;
        bufv.resize(6,0);
        gve1.resize(gn+1,bufv);
    }
    gve1[1][0] = 1;
    for(int i = 2;i<=gn;i++){
        vector<long long> &b = gve1[i-1];
        gve1[i][0] = (b[0]*1)%gmod;
        gve1[i][1] = (b[0]*1+b[1]*2)%gmod;
        gve1[i][2] = (b[1]*1+b[2]*2)%gmod;
        gve1[i][3] = (b[1]*1+b[3]*2+b[5]*1)%gmod;
        gve1[i][4] = (b[2]*1+b[3]*1+b[4]*2)%gmod;
        gve1[i][5] = (b[0]*1+b[5]*1)%gmod;
    }
    cout << gve1[gn][4] << endl;
    return 0;
}
