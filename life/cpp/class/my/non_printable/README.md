#include<iostream>
#include<string>
using namespace std;

int main(){
    string s;
    while(getline(cin,s)){
        for(int i = 0;i<s.size();i++){
            if(s[i] != '\n' && s[i] != '\r' && s[i] != '\0' && (s[i] < ' ' || s[i] > 0x7F)){
                s[i] = ' ';
            }
        }
        cout << s << "\n";
    }
    return 0;
}
