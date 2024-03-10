//#define YLFILE
//#define YWATCH
#include<iostream>
#include<iomanip>
#include<string>
#include<sstream>
#include<fstream>
#include<vector>
#include<list>
#include<map>
#include<algorithm>
#include<cmath>
/*for built-in type(except c_array) & map & list & vector*/
class YPrint{
public:
    static int indent_;
    template<typename T> static void W(const std::string &info, const T &x, bool clr = false) {
        #ifdef YWATCH
        if (clr) system("clear"); //system("cls"); in windows
        std::cout << "\n\\**********************\n" << info << "\n" << x << "**********************\\\n";
        #endif
    }
};
int YPrint::indent_ = 0;
#define YOVERLOAD typename T> std::ostream &operator << (std::ostream &os, const std::
#define YPRINT(...) YOVERLOAD __VA_ARGS__ > &st){\
    YPrint::indent_++;\
    os << "OBJECT(size=" << st.size() << ")\n";\
    typename std:: __VA_ARGS__ >::const_iterator it = st.begin();\
    for (size_t i = 0; i < st.size(); i++, it++){\
        for(int j=0;j<YPrint::indent_;j++){os << "\t";}\
        os << "[" << i << "]=" << *it << "*\n";\
    }YPrint::indent_--;\
    return os;\
}
template<typename S, YOVERLOAD pair<S, T> &st) { return os << "{" << st.first << "}" << st.second; }
template<typename S, YPRINT(map<S, T)
template<YPRINT(list<T)
template<YPRINT(vector<T)
int main() {
    std::cin.tie(0);
    #ifdef YLFILE
    std::cin.rdbuf((new std::ifstream("yin1.txt"))->rdbuf());
    //std::cout.rdbuf((new std::ofstream("yout1.txt"))->rdbuf());
    #endif
    int n = 0;
    int k = 0;
    std::cin >> n >> k;
    int bit_of_one = 0;
    std::vector<int> num_of_every_bit;//this[0]:num_of 2^0
    num_of_every_bit.resize(31,0);
    int bufn = n;
    for(int i = 0;bufn!=0;bufn >>= 1,i++){
        if(bufn&1){
            bit_of_one++;
            num_of_every_bit[i] = 1;
        }
    }
    YPrint::W("bit_of_one",bit_of_one);
    //YPrint::W("num_of_every_bit",num_of_every_bit);
    YPrint::W("k",k);
    YPrint::W("n",n);
    if(bit_of_one <= k && k <= n){
        std::cout << "YES" << "\n";
        int index = 30;
        while(num_of_every_bit[index] == 0){
            index--;
        }
        while(bit_of_one < k){
            num_of_every_bit[index]--;
            num_of_every_bit[index-1]+=2;
            bit_of_one++;    
            if(num_of_every_bit[index] == 0){
                index--;
            }
        }
        for(int i = 0;i<31;i++){
            for(int j = 0;j<num_of_every_bit[i];j++){
                std::cout << (1<<(i)) << " ";
            }
        }
    }else{
        std::cout << "NO" << "\n";
    }
    return 0;
}

