#include <iostream>  
  
using namespace std;  
  
int main(){  
    int mod = 1000000007;  
    int n;  
    cin>>n;  
    long long **states = new long long*[n+1];  
    for(int i =0;i<n+1;i++)  
        states[i]=new long long[6];  
    for(int i =0;i<6;i++)  
        states[0][i]=0;  
  
    for(int i=1;i<=n;i++)  
    {  
        int j = i-1;  
        states[i][0] = 1;  
        states[i][1] = (states[j][0] + states[j][1] * 2) % mod;  
        states[i][2] = (states[j][0] + states[j][2]) % mod;  
        states[i][3] = (states[j][1] + states[j][3] * 2) % mod;  
        states[i][4] = (states[j][1] + states[j][2] + states[j][4] * 2) % mod;  
        states[i][5] = (states[j][3] + states[j][4] + states[j][5] * 2) % mod;  
    }  
    cout<<states[n][5]<<endl;  
    return 0;  
} 
