#include<iostream>
#include<vector>
#include<map>
#include<algorithm>
using namespace std;
class ycop{
public:
    ycop(int w,int n,bool o):when(w),no(n),rop(o){}
    int when;
    int no;
    bool rop;//true:return false:borrow
};
bool operator<(const ycop&y1,const ycop&y2){
    if(y1.when < y2.when){
        return true;
    } else if(y1.when == y2.when){
        if(y1.rop > y2.rop){
            return true;
        } else if(y1.rop == y2.rop){
            if(y1.no < y2.no){
                return true;
            }
        }
    }
    return false;
}
int main(){
    cin.tie(0);
    vector<int> ltk;//ve[loc](0s) = key(0s)
    vector<int> ktl;//ve[key](0s) = loc(0s)
    vector<ycop> ret;
    map<int,int> emploc;//loc(0s) count
    int gk = 0;
    int gn = 0;
    cin >> gk >> gn;
    ltk.reserve(gk);
    ktl.reserve(gk);
    ret.reserve(gn);
    for(int i = 0;i<gk;i++){
        ktl.push_back(i);
        ltk.push_back(i);
    }
    for(int i = 0;i<gn;i++){
        int k = 0;
        int s = 0;
        int l = 0;
        cin >> k >> s >> l;
        k--;
        ret.push_back(ycop(s,k,false));
        ret.push_back(ycop(s+l,k,true));
    }
    sort(ret.begin(),ret.end());
    int rsize = ret.size();
    for(int i = 0;i<rsize;i++){
        ycop &y = ret[i];
        if(y.rop){
            map<int,int>::iterator itm = emploc.begin();
            int eloc = itm->first;
            if(itm->second > 1){
                itm->second--;
            } else{
                emploc.erase(itm);
            }
            ltk[eloc] = y.no;
            ktl[y.no] = eloc;
        } else{
            int loc = ktl[y.no];
            emploc[loc]++;
        }
    }
    for(int i = 0;i<gk;i++){
        cout << ltk[i]+1 << " ";
    }
    return 0;
}

