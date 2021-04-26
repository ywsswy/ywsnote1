#include <iostream>
#include <cstdlib>
#include <iomanip>
#include <string>
#include <sstream>
#include <fstream>
#include <vector>
#include <list>
#include <map>
#include <algorithm>
#include <cmath>
class YPrint {
public:
    static int indent_;
    template<typename T> static void W(const std::string &info, const T &x, bool clr = false) {
#ifdef YWATCH
        if (clr) system("clear"); //system("cls"); in windows
        std::cout << "\n\\******************\n" << info << "\n" << x << "******************\\\n";
#endif
    }
};
int YPrint::indent_ = 0;
#define YOVERLOAD typename T> std::ostream &operator << (std::ostream &os, const std::
#define YPRINT(...) YOVERLOAD __VA_ARGS__ > &st) {\
    ++YPrint::indent_;\
    os << "OBJECT(size=" << st.size() << ")\n";\
    typename std:: __VA_ARGS__ >::const_iterator it = st.begin();\
    for (size_t i = 0; i < st.size(); i++, it++) {\
        for(int j=0;j<YPrint::indent_;j++) os << "\t";\
        os << "[" << i << "]=" << *it << "*\n";\
    } --YPrint::indent_;\
    return os;\
}
template<typename S, YOVERLOAD pair<S, T> &st) { return os << "{" << st.first << "}" << st.second; }
template<typename S, YPRINT(map<S, T)
template<YPRINT(list<T)
template<YPRINT(vector<T)

int main() {
  std::cin.tie(0);
  std::ios::sync_with_stdio(false);
  return 0;
}

