class YMat{
public:
    YMat(int n,long long mod):n_(n),mod_(mod){
        std::vector<long long> buf;
        buf.resize(n,0);
        data_.resize(n,buf);
    }
    YMat operator *(const YMat &y){
        YMat res(y.n_,y.mod_);
        for(int i = 0;i<y.n_;i++){
            for(int j = 0;j<y.n_;j++){
                for(int k = 0;k<y.n_;k++){
                    res.data_[i][j] += data_[i][k] * y.data_[k][j];
                }
                res.data_[i][j] %= mod_;
            }
        }
        return res;
    }
    std::vector<std::vector<long long> > data_;
    int n_;
    long long mod_;
};
YMat PowModMat(YMat &x,long long n) {
    //矩阵则构造一个初始的单位矩阵E，普通数则初始为1
    YMat res(x.n_,x.mod_);
    for(int i = 0;i<x.n_;i++){
        res.data_[i][i] = 1;
    }
    while(n){
    //例如A^9 = A^(1001)(2) = E*A*A^8 = E*A*(A^2)^4 = ...
        if(n&1){
            res = res*x;
        }
        x = x*x;
    	n >>= 1;
    }
    return res;
}
    const long long kMod = 65536;
    const int kN = 3;
    YMat p(kN,kMod);
    YMat pn = PowModMat(p,n-2);
