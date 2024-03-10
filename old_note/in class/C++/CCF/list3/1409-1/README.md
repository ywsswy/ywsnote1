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
    ios::sync_with_stdio(false);
    cin.tie(0);
#ifdef YLOFI
    freopen("yin1.txt", "r", stdin);
    //freopen("yout1.txt","w",stdout);
#endif
#ifdef YDELO
#endif
    int gn = 0;
    cin >> gn;
    map<int, int> gma1;
    for (int i = 0; i < gn; i++) {
        int buf1 = 0;
        cin >> buf1;
        yCrossMerge(gma1, buf1, buf1);
    }
    int couple = 0;
    {
        map<int, int>::iterator itm = gma1.begin();
        int size = gma1.size();
        for(int i = 0;i<size;i++,itm++){
            couple += (itm->second - itm->first);
        }
    }
    cout << couple << endl;
    return 0;
}
