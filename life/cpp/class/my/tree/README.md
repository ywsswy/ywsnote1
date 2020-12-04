#include <iostream>
#include <unistd.h>
#include <cstdlib>
#include <fstream>
#include <map>
#include <list>
#include <vector>
#include <dirent.h>
class YPrint
{
public:
    static int indent_;
    template<typename T> static void W(const std::string &info, const T &x, bool clr = false)
    {
#ifdef YWATCH
        if (clr) system("clear"); //system("cls"); in windows
        std::cout << "\n\\******************\n" << info << "\n" << x << "*\\\n";
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

class MyTree{
public:
    std::map<std::string, MyTree> tree;
};

std::ostream &operator<<(std::ostream &os,const MyTree &st){
    os << "tree:" << st.tree;
    return os;
}

void GetTree(const std::string &path, MyTree &mytree)
{
    DIR *dir;
    struct dirent *dir_data;
    if ((dir = opendir(path.data())) != NULL)
    {
        while ((dir_data = readdir(dir)) != NULL)
        {
            MyTree buf;
            std::string next_path = path + "/" + dir_data->d_name;
            if (dir_data->d_type == DT_DIR ||
                dir_data->d_type == DT_LNK)
            {
                if (std::string(dir_data->d_name) != "." &&
                    std::string(dir_data->d_name) != "..")
                {
                    GetTree(next_path, buf);
                    mytree.tree[dir_data->d_name] = buf;
                }
            }
            else
            {
                mytree.tree[dir_data->d_name] = buf;
            }
        }
        return;
    }
    closedir(dir);
}
void PrintTree(MyTree &mytree)
{
    static int indent = 0;
    int j = 0;
    for (std::map<std::string, MyTree>::iterator it = mytree.tree.begin(); it != mytree.tree.end(); it++, ++j)
    {
        for (int i = 0; i < indent; ++i)
        {
            if (i == indent - 1)
            {
                if (j + 1 == mytree.tree.size())
                {
                    std::cout << "└── ";
                }
                else
                {
                    std::cout << "├── ";
                }
            }
            else
            {
                std::cout << "│   ";
            }
        }
        std::cout << it->first << std::endl;
        ++indent;
        PrintTree(it->second);
        --indent;
    }
}

class Args
{
public:
    Args() : dirname(".") {}
    std::string dirname;
};

bool help()
{
    std::cout << "usage: ./main [-h] [-d dirname(couldn't start with '~')]" << std::endl;
    return false;
}
bool ParseArgs(int argc, char* argv[], Args &info)
{
    int ch;
    while ((ch = getopt(argc, argv, "d:h")) != -1)
    {
        switch (ch)
        {
        case 'd':
            info.dirname = optarg;
            break;
        case 'h':
            return help();
        default:
            return help();
        }
    }
    return true;
}

int main(int argc, char* argv[])
{
    Args info;
    if (!ParseArgs(argc, argv, info))
    {
        exit(-1);
    }
    MyTree mytree;
    mytree.tree[info.dirname];
    GetTree(info.dirname, mytree.tree[info.dirname]);
    PrintTree(mytree);
    return 0;
}