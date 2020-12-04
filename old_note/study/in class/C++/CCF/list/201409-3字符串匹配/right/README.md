#include<iostream>
#include<ctype.h>
#include<string>
using namespace std;
int main(){
	freopen("in2.txt","r",stdin);
	string sor;
	cin >> sor;
	int flag;
	cin >> flag;
	if(flag == 0){		
		for(int i = 0;i<sor.length();i++){
			sor[i] = toupper(sor[i]);
		}
	}
	int n;
	cin >> n;
	for(int i = 0;i<n;i++){
		string obuf;
		string buf;
		cin >> obuf;
		buf = obuf;
		if(flag == 0){
			for(int j = 0;j<buf.length();j++){
				buf[j] = toupper(buf[j]);
			}
		}
		if(buf.find(sor) != -1){
			cout << obuf << endl;
		}
	}
	return 0;
}
