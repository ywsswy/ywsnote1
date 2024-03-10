#include<iostream>
#include<cstdarg>
#include<cstdio>
#include<string>
#include<cassert>
void fun(const char* format, ...){
    char info[20] = {0};
    va_list args;
    va_start(args, format);    
    vsprintf(info,format,args);
    va_end(args); 
    std::string s(info);
    if(sizeof(info) <= s.size()){
        assert("parameter too long");
    }   
    std::cout << s << '\n';
}
int main(){
    fun("%s%d\n","789012",34);
    return 0;
}
