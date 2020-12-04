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
class ycnode {
public:
    string name;
    list<int> near;
    bool visit;
    //others
    int lay;
    ycnode() : visit(false), lay(0) {
    }
};
#ifdef YDELO
int ydelon = 0;
/*for built-in type(except c_array) & map & list & vector*/
template<typename T>
void yPrint(const string &info, const T &x, int n = 0, bool clr = true) {
    ydelon = n;
    if (clr) {
        system("clear"); //system("cls");
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
    size_t n = (ydelon == 0 ? st.size() : ydelon);
    os << " size=" << st.size() << " show=" << n << endl;
    typename map<T, S>::const_iterator it = st.begin();
    for (size_t i = 0; i < n && i < st.size(); i++, it++) {
        os << i << " " << *it << "*" << endl;
    }
    return os;
}
template<typename T>
ostream &operator<<(ostream &os, const list<T> &st) {
    size_t n = (ydelon == 0 ? st.size() : ydelon);
    os << " size=" << st.size() << " show=" << n << endl;
    typename list<T>::const_iterator it = st.begin();
    for (size_t i = 0; i < n && i < st.size(); i++, it++) {
        os << i << " " << *it << "*" << endl;
    }
    return os;
}
template<typename T>
ostream &operator<<(ostream &os, const vector<T> &st) {
    size_t n = (ydelon == 0 ? st.size() : ydelon);
    os << " size=" << st.size() << " show=" << n << endl;
    typename vector<T>::const_iterator it = st.begin();
    for (size_t i = 0; i < n && i < st.size(); i++, it++) {
        os << i << " " << *it << "*" << endl;
    }
    return os;
}
ostream &operator<<(ostream &os, const ycnode &yc) {
    os << "name=" << yc.name
            << ",visit" << yc.visit
            << ",lay" << yc.lay
            << ",near=(" << yc.near << ")";
    return os;
}
#endif
void yIntToString(const int &n, string &s) {
    stringstream ss;
    ss << n;
    ss >> s;
    return;
}
class yG {
public:
    map<string, int> ma; //name->id
    vector<ycnode> ve; //id(0s)->node
    int id = -1; //maxid(ed)
    void init(const int gn, const int gm);//create graph
    void bfs(const int start,const int type,int &par);
};
void yG::init(const int gn, const int gm) {
    this->ve.resize(gn + gm, ycnode());
    this->id++;
    this->ma["s1"] = this->id;
    this->ve[this->id].name = "s1";
    for (int i = 0; i < gn - 1; i++) {
        this->id++;
        string bufname;
        yIntToString(i + 2, bufname);
        this->ma["s" + bufname] = this->id;
        this->ve[this->id].name = "s" + bufname;
        int near = 0;
        cin >> near;
        this->ve[this->id].near.push_back(near - 1);
        this->ve[near - 1].near.push_back(this->id);
    }
    for (int i = 0; i < gm; i++) {
        this->id++;
        string bufname;
        yIntToString(i + 1, bufname);
        this->ma["p" + bufname] = this->id;
        this->ve[this->id].name = "p" + bufname;
        int near = 0;
        cin >> near;
        this->ve[this->id].near.push_back(near - 1);
        this->ve[near - 1].near.push_back(this->id);
    }
}
void yG::bfs(const int start,const int type,int &par){
    /*type:par
     0:gmaxfarid
     1:gmaxstep*/
    list<int> q;
    q.push_back(start);
    this->ve[start].visit = true;
    while (!q.empty()) {
        int head = q.front();
        q.pop_front();
        if(type == 0){//deal
            par = head;
        } else if(type == 1){
            par = this->ve[head].lay;
        }
        list<int>::iterator itl = this->ve[head].near.begin();
        size_t size = this->ve[head].near.size();
        for (size_t i = 0; i < size; i++, itl++) {
            if (!this->ve[*itl].visit) {
                this->ve[*itl].visit = true;
                q.push_back(*itl);
                if(type == 1){//deal
                    this->ve[*itl].lay = this->ve[head].lay + 1;
                }
            }
        }
    }
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
    int gn = 0; //switch
    int gm = 0; //pc
    cin >> gn >> gm;
    yG g1;
    g1.init(gn, gm);
    //first_bfs
    int gmaxfarid = -1;
    g1.bfs(0,0,gmaxfarid);
    //second bfs
    {
        size_t size1 = g1.ve.size();
        for (size_t i = 0; i < size1; i++) {
            g1.ve[i].visit = false;
        }
    }
    int gmaxstep = 0;
    g1.bfs(gmaxfarid,1,gmaxstep);
    cout << gmaxstep << endl;
    return 0;
}
