#define YLFILE
#define YWATCH
#define YINDEX(x) (x-1)
#include <iostream>
#include <iomanip>
#include <vector>
#include <fstream>
#include <list>
#include <map>
#include <algorithm>
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
bool JudgeTotal(std::vector<std::vector<int> > &ve, const int turn){
  for(int i = 1;i<=3;++i){
    if(turn == ve[YINDEX(i)][YINDEX(1)] && turn == ve[YINDEX(i)][YINDEX(2)] && turn == ve[YINDEX(i)][YINDEX(3)]){
      return true;
    }
    if(turn == ve[YINDEX(1)][YINDEX(i)] && turn == ve[YINDEX(2)][YINDEX(i)] && turn == ve[YINDEX(3)][YINDEX(i)]){
      return true;
    }
  }
  if(turn == ve[YINDEX(1)][YINDEX(1)] && turn == ve[YINDEX(2)][YINDEX(2)] && turn == ve[YINDEX(3)][YINDEX(3)]){
    return true;
  }
  if(turn == ve[YINDEX(1)][YINDEX(3)] && turn == ve[YINDEX(2)][YINDEX(2)] && turn == ve[YINDEX(3)][YINDEX(1)]){
    return true;
  }
  return false;
}
int Space(std::vector<std::vector<int> > &ve){
  int count = 0;
  for(int i = 1;i<=3;++i){
    for(int j = 1;j<=3;++j){
      if(ve[YINDEX(i)][YINDEX(j)] == 0){
        ++count;
      }
    }
  }
  return count;
}
class YStep{
 public:
  int c_;
  int r_;
  int score_; 
  int old_color_;
  bool operator<(const YStep &yc1){
    return this->score_ < yc1.score_;
  }
};
std::ostream& operator<<(std::ostream& os, const YStep& y){
  return os << '(' << y.r_ << ',' << y.c_ << "): " << y.score_;
}
bool JudgePoint(std::vector<std::vector<int> > &ve, const int turn, const int r, const int c){
  if(turn == ve[YINDEX(r)][YINDEX(1)] && turn == ve[YINDEX(r)][YINDEX(2)] && turn == ve[YINDEX(r)][YINDEX(3)]){
    return true;
  }
  if(turn == ve[YINDEX(1)][YINDEX(c)] && turn == ve[YINDEX(2)][YINDEX(c)] && turn == ve[YINDEX(3)][YINDEX(c)]){
    return true;
  }
  if((r+c)%2 == 0){
    if(turn == ve[YINDEX(1)][YINDEX(1)] && turn == ve[YINDEX(2)][YINDEX(2)] && turn == ve[YINDEX(3)][YINDEX(3)]){
      return true;
    }
    if(turn == ve[YINDEX(1)][YINDEX(3)] && turn == ve[YINDEX(2)][YINDEX(2)] && turn == ve[YINDEX(3)][YINDEX(1)]){
      return true;
    }
  }
  return false;
}
bool CanPut(std::vector<std::vector<int> > &ve, const int r, const int c){
  if(ve[YINDEX(r)][YINDEX(c)] == 0){
    return true;
  } else {
    return false;
  }
}
void Store(std::vector<YStep> &change_back, std::vector<std::vector<int> > &ve,const int r, const int c, const int turn){
  {
    YStep buf;
    buf.r_ = r;
    buf.c_ = c;
    buf.old_color_ = ve[YINDEX(r)][YINDEX(c)];
    change_back.push_back(buf);
    ve[YINDEX(r)][YINDEX(c)] = turn;
  }
  return;
}
void Restore(std::vector<YStep> &change_back, std::vector<std::vector<int> > &ve){
  size_t size = change_back.size();
  std::vector<YStep>::iterator it = change_back.begin();
  for(size_t i = 1;i<=size;++i,++it){
    ve[YINDEX(it->r_)][YINDEX(it->c_)] = it->old_color_;
  }
  return;
}
int Cal(std::vector<std::vector<int> > &ve, const int turn){
  std::vector<YStep> choice;
  for(int i = 1;i<=3;++i){
    for(int j = 1;j<=3;++j){
      if(CanPut(ve,i,j)){
        std::vector<YStep> change_back;//store changed points of this choice point
        Store(change_back,ve,i,j,turn);
        YStep bufc;
        bufc.r_ = i;
        bufc.c_ = j;
        if(JudgePoint(ve,turn,i,j)){
          Restore(change_back,ve);
          return Space(ve) * (-2 * turn + 3);
        } else {
          bufc.score_ = Cal(ve,3-turn);
          Restore(change_back,ve);
          choice.push_back(bufc);
        }
      }
    }
  }
  if(choice.size() == 0){
    return 0;
  } else {
    sort(choice.begin(), choice.end());
    if(turn == 1){
      return choice.rbegin()->score_;
    } else {
      return choice.begin()->score_;
    }
  }
}
class Game{
 public:
  int w_;
  int h_;
  void Init();
};
int main(){
  std::cin.tie(0);
  std::ios::sync_with_stdio(false);
#ifdef YLFILE
  std::cin.rdbuf((new std::ifstream("yin1.txt"))->rdbuf());
#endif
  int gn = 0;
  std::cin >> gn;
  for(int gi = 1;gi<=gn;++gi){
    std::vector<std::vector<int> > ve(3,std::vector<int>(3,0));
    for(int i = 1;i<=3;++i){
      for(int j = 1;j<=3;++j){
        std::cin >> ve[YINDEX(i)][YINDEX(j)];
      }
    }
    if(JudgeTotal(ve,1)){
      std::cout << Space(ve)+1 << '\n';
    } else if(JudgeTotal(ve,2)){
      std::cout << -(Space(ve)+1) << '\n';
    } else {
      std::cout << Cal(ve,1) << '\n';
    }
  }
  return 0;
}

