//#define YLFILE
#define YWATCH
#define YINDEX(x) (x-1)
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
    std::cin >> n;
    std::vector<std::vector<int> > ve;
    ve.reserve(n);
    {
        std::vector<int> vebuf;
        vebuf.resize(3,0);
        for(int i = 1;i<=n;i++){
            std::cin >> vebuf[YINDEX(1)] >> vebuf[YINDEX(2)];
            vebuf[YINDEX(3)] = vebuf[YINDEX(1)] + vebuf[YINDEX(2)];
//            YPrint::W("vebuf",vebuf);
            ve.push_back(vebuf);
        }
    }
//    YPrint::W("ve",ve);
    //assume i1 -> ve[i1][i1] -> ve[i1][i2]
    int num_after_first = ve[YINDEX(1)][YINDEX(1)];
    if(ve[YINDEX(num_after_first)][YINDEX(1)] == ve[YINDEX(1)][YINDEX(2)]
        ||
        ve[YINDEX(num_after_first)][YINDEX(2)] == ve[YINDEX(1)][YINDEX(2)]){
        //assume right.
    }else{
        num_after_first = ve[YINDEX(1)][YINDEX(2)];
    }
    int former = 1;
    int latter = num_after_first;
    for(int i = 1;i<=n;i++){
        std::cout << former << ' ';
        former = ve[YINDEX(former)][YINDEX(3)] - latter;
        std::swap(former,latter);
    }
    return 0;
}

