//快速幂
int yPowMod(long long x,long long n,int mod) {
    int res = 1;
    while(n){
        if(n&1){
			res = x*res%mod;
		}
		x = x*x%mod;
    	n >>= 1;
    }
    return res;
}
