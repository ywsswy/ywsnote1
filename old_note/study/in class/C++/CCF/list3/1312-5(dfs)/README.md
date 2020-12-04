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
class yc2i{
public:
    int i1;
    int i2;
    yc2i(int a=0,int b=0):i1(a),i2(b){}
};
ostream &operator<<(ostream &os,const yc2i &yc){
    os << "i1=" << yc.i1 << ",i2=" << yc.i2;
    return os;
}
bool canup(const yc2i &head,vector<string> &gplat){
    if(head.i2 == 0){
        return false;
    } else{
        char buf = gplat[head.i2][head.i1];
        if(buf == '-' || buf == '.' || gplat[head.i2-1][head.i1]=='#'){
            return false;
        }
    }
    return true;
}
bool candown(const yc2i &head,vector<string> &gplat,const int gr){
    if(head.i2 == gr-1){
        return false;
    } else{
        char buf = gplat[head.i2][head.i1];
        if(buf == '-' || gplat[head.i2+1][head.i1]=='#'){
            return false;
        }
    }
    return true;
}
bool canleft(const yc2i &head,vector<string> &gplat){
    if(head.i1 == 0){
        return false;
    } else{
        char buf = gplat[head.i2][head.i1];
        if(buf == '|' || buf == '.' || gplat[head.i2][head.i1-1]=='#'){
            return false;
        }
    }
    return true;
}
bool canright(const yc2i &head,vector<string> &gplat,const int gc){
    if(head.i1 == gc-1){
        return false;
    } else{
        char buf = gplat[head.i2][head.i1];
        if(buf == '|' || buf == '.' || gplat[head.i2][head.i1+1]=='#'){
            return false;
        }
    }
    return true;
}
void dfs1(list<yc2i> &q,vector<string> &gplat,const int gr,const int gc,vector<vector<bool> > &gvis){
    yc2i head = q.front();
    if(canup(head,gplat) && !gvis[head.i2-1][head.i1]){
        q.push_back(yc2i(head.i1,head.i2-1));
        gvis[head.i2-1][head.i1] = true;
        return;
    }
    if(candown(head,gplat,gr) && !gvis[head.i2+1][head.i1]){
        q.push_back(yc2i(head.i1,head.i2+1));
        gvis[head.i2+1][head.i1] = true;
        return;
    }
    if(canleft(head,gplat) && !gvis[head.i2][head.i1-1]){
        q.push_back(yc2i(head.i1-1,head.i2));
        gvis[head.i2][head.i1-1] = true;
        return;
    }
    if(canright(head,gplat,gc) && !gvis[head.i2][head.i1+1]){
        q.push_back(yc2i(head.i1+1,head.i2));
        gvis[head.i2][head.i1+1] = true;
        return;
    }
    q.pop_front();
    return;
}
bool canup2(const yc2i &head,vector<string> &gplat){
    if(head.i2 == 0){
        return false;
    } else{
        char buf = gplat[head.i2-1][head.i1];
        if(buf == '-' || buf == '#'){
            return false;
        }
    }
    return true;
}
bool candown2(const yc2i &head,vector<string> &gplat,const int gr){
    if(head.i2 == gr-1){
        return false;
    } else{
        char buf = gplat[head.i2+1][head.i1];
        if(buf == '-' || buf == '.' || buf == '#'){
            return false;
        }
    }
    return true;
}
bool canright2(const yc2i &head,vector<string> &gplat,const int gc){
    if(head.i1 == gc-1){
        return false;
    } else{
        char buf = gplat[head.i2][head.i1+1];
        if(buf == '|' || buf == '.' || buf == '#'){
            return false;
        }
    }
    return true;
}
bool canleft2(const yc2i &head,vector<string> &gplat){
    if(head.i1 == 0){
        return false;
    } else{
        char buf = gplat[head.i2][head.i1-1];
        if(buf == '|' || buf == '.' || buf == '#'){
            return false;
        }        
    }
    return true;
}
void dfs2(list<yc2i> &q,vector<string> &gplat,const int gr,const int gc,vector<vector<bool> > &gvis){
    yc2i head = q.front();
    if(canup2(head,gplat) && !gvis[head.i2-1][head.i1]){
        q.push_back(yc2i(head.i1,head.i2-1));
        gvis[head.i2-1][head.i1] = true;
        return;
    }
    if(candown2(head,gplat,gr) && !gvis[head.i2+1][head.i1]){
        q.push_back(yc2i(head.i1,head.i2+1));
        gvis[head.i2+1][head.i1] = true;
        return;
    }
    if(canleft2(head,gplat) && !gvis[head.i2][head.i1-1]){
        q.push_back(yc2i(head.i1-1,head.i2));
        gvis[head.i2][head.i1-1] = true;
        return;
    }
    if(canright2(head,gplat,gc) && !gvis[head.i2][head.i1+1]){
        q.push_back(yc2i(head.i1+1,head.i2));
        gvis[head.i2][head.i1+1] = true;
        return;
    }
    q.pop_front();
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
    int gr = 0;
    int gc = 0;
    cin >> gr >> gc;
    vector<string> gplat;
    gplat.resize(gr,"");
    int gsx = 0;
    int gsy = 0;
    int gex = 0;
    int gey = 0;
    //input
    {
        bool flags = false;
        bool flage = false;
        for(int i = 0;i<gr;i++){
            cin >> gplat[i];
            if(!flags){
                size_t fi = gplat[i].find('S');
                if(fi != string::npos){
                    flags = true;
                    gsx = fi;
                    gsy = i;
                }
            }
            if(!flage){
                size_t fi = gplat[i].find('T');
                if(fi != string::npos){
                    flage = true;
                    gex = fi;
                    gey = i;
                }
            }
        }
    }
    
    //bfs1
    vector<vector<bool> > gvis1;
    {
        vector<bool> buf;
        buf.resize(gc,false);
        gvis1.resize(gr,buf);
    }
    list<yc2i> gq1;
    gq1.push_back(yc2i(gsx,gsy));
    gvis1[gsy][gsx] = true;
    while(!gq1.empty()){
        dfs1(gq1,gplat,gr,gc,gvis1);
    }
    if(!gvis1[gey][gex]){
        cout << "I'm stuck!" << endl;
        return 0;
    }
    //bfs2
    vector<vector<bool> > gvis2;
    {
        vector<bool> buf;
        buf.resize(gc,false);
        gvis2.resize(gr,buf);
    }
    list<yc2i> gq2;
    gq2.push_back(yc2i(gex,gey));
    gvis2[gey][gex] = true;
    while(!gq2.empty()){
        dfs2(gq2,gplat,gr,gc,gvis2);
    }
    int gcount = 0;
    for(int i = 0;i<gr;i++){
        for(int j = 0;j<gc;j++){
            if(gvis1[i][j] && !gvis2[i][j]){
                gcount++;
            }
        }
    }
    cout << gcount << endl;
    return 0;
}
