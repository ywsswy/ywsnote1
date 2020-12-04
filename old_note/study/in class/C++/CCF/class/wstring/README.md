#include <iostream>
#include <string>
#include <fstream>
#include <sstream>
#include <vector>
void ReadCsFromFile(const std::string &file_name, std::vector<std::wstring> &vec_cs){
    std::wstring line = L"";
    std::wifstream if1(file_name.c_str());
    while(getline(if1,line)){
        vec_cs.push_back(line);
    }
    return;
}
int main(){
    std::vector<std::wstring> vec_cs;
    ReadCsFromFile("raw_cs.txt",vec_cs);
    return 0;
}
