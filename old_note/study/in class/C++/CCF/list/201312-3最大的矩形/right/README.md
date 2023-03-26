#include<iostream>
#include<map>
#include<algorithm>
using namespace std;
int main(){
    //freopen("in2.txt","r",stdin);
	int gn = 0;
    cin >> gn;
    map<int,int> ma;
    for(int i = 0;i<gn;i++){
        int buf = 0;
        cin >> buf;
        ma[i] = buf;
    }
    int gmax = 0;
    for(map<int,int>::iterator itm = ma.begin();itm != ma.end();itm++){
        int high = itm->second;
        int maxlen = 0;
        int len = 0;
        for(int j = 0;j<gn;j++){
            if(ma[j] >= high){
                len++;
                if(len>maxlen){
                    maxlen = len;
                }
            }
            else{
                len = 0;
            }
        }
        gmax = max(gmax,high*maxlen);
    }
    cout << gmax << endl;
    return 0;
}
