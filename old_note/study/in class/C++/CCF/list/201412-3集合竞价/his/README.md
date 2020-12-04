#include<iostream>  
#include<set>  
#include<string>
using namespace std;  
#define MAX 5007  
struct R{  
    string m;  //cmd
    double p;  
    int s;  
    bool e;  //有效性
};
R r[MAX];  
set<double> st;  //单价空间上的集合
int main(){
	freopen("in2.txt","r",stdin);
    int m=0;  //语条数
    while(cin>>r[m].m){     
        if(r[m].m == "cancel"){  
            cin>>r[m].s;  
            r[r[m].s-1].e=true;  
            m++;  
            continue;  
        }  
        cin>>r[m].p>>r[m].s;  
        ++m;  
    }
    for(int i=0;i<m;++i)  
        if(r[i].m != "cancel" && !r[i].e)st.insert(r[i].p);  
      
    long long m_sum=0;  
    double p=0;  
    for(set<double>::iterator it=st.begin();it!=st.end();++it){  
        double p0 = *it;  
        long long sumb=0,sums=0,sum;  
        for(int i=0;i<m;++i)  
            if(r[i].m == "sell" && !r[i].e && r[i].p<=p0)
				sums+=r[i].s;  
        for(int i=0;i<m;++i)  
            if(r[i].m == "buy" && !r[i].e && r[i].p>=p0)
				sumb+=r[i].s;  
        sum=min(sums,sumb);  
          
        if(sum>=m_sum){  
            m_sum=sum;  
            p=p0;  
        }  
    }  
	cout.setf(ios::fixed);
	cout.precision(2);
	cout << p << " " << m_sum; 
    return 0;  
}
