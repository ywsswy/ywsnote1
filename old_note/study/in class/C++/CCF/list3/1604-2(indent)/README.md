#define YLFILE
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
#define YOVERLOAD std::ostream &operator << (std::ostream &os, const
#define YGETSIZE > &st){yindent++;os << "OBJECT(size=" << st.size() << ")\n";typename
#define YOUTPUT >::const_iterator it = st.begin();for (size_t i = 0; i < st.size(); i++, it++){for(int j=0;j<yindent;j++){os << "\t";}os << "[" << i << "]=" << *it << "*\n";}yindent--;return os;}
int yindent = 0;
template<typename T>
void yPrint(const std::string &info, const T &x, bool clr = true) {
    if (clr) system("clear"); //system("cls"); in windows
    std::cout << "\n\\**********************\n" << info << "\n" << x << "**********************\\\n";
}
template<typename T, typename S>
YOVERLOAD std::pair<T, S> &st) { return os << "{" << st.first << "}" << st.second; }
template<typename T, typename S>
YOVERLOAD std::map<T, S YGETSIZE std::map<T, S YOUTPUT
template<typename T>
YOVERLOAD std::list<T YGETSIZE std::list<T YOUTPUT
template<typename T>
YOVERLOAD std::vector<T YGETSIZE std::vector<T YOUTPUT
int main() {
    std::cin.tie(0);
    #ifdef YLFILE
    std::cin.rdbuf((new std::ifstream("yin1.txt"))->rdbuf());
    //std::cout.rdbuf((new std::ofstream("yout1.txt"))->rdbuf());
    #endif
    std::vector<std::string> ve;
    ve.resize(10,"");
    for(int i = 0;i<15;i++){
        for(int j = 0;j<10;j++){
            std::string s = "";
            std::cin >> s;
            ve[j] += s;
        }
    }
    std::vector<std::string> ve2;
    ve2.resize(4,"");
    for(int i = 0;i<4;i++){
        for(int j = 0;j<4;j++){
            std::string s = "";
            std::cin >> s;
            ve2[j] += s;
        }
    }
    int col = 0;
    std::cin >> col;
    col--;
    std::vector<size_t> fi2;
    for(int i = 0;i<4;i++){
        size_t loc = ve2[i].rfind("1");
        fi2.push_back(loc);
    }
    std::vector<size_t> fi1;
    for(int i = 0+col;i<4+col;i++){
        size_t loc = ve[i].find("1");
        if(loc == std::string::npos){
            loc = 15;
        }
        fi1.push_back(loc);
    }
    int nearest = 99;//(0s)
    for(int i = 0;i<4;i++){
        if(fi2[i] != std::string::npos){
            int dis = fi1[i]-fi2[i];
            nearest = std::min(nearest,dis);
        }
    }
    for(int i = 0;i<4;i++){
        for(int j = 0;j<4;j++){
            if(ve2[i][j] == '1'){
                int dest = nearest-1+j;
                if(dest >= 0){
                    ve[i+col][dest] = '1';
                }
            }
        }
    }
    for(int i = 0;i<15;i++){
        for(int j = 0;j<10;j++){
            std::cout << ve[j][i] << " ";
        }
        std::cout << "\n";
    }
    #ifdef YWATCH
    yPrint("ve",ve,false);
    yPrint("ve2",ve2,false);
    yPrint("fi1",fi1,false);
    yPrint("fi2",fi2,false);
    #endif
    return 0;
}
