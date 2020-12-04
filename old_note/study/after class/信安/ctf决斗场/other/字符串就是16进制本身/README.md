#include<iostream>
using namespace std;
int ych(char c){
	if(c >= '0' && c <= '9'){
		return c-'0';
	}
	else{
		return c-'A'+10;
	}
}
int main(){
	freopen("in.txt","r",stdin);
	char bufc = 0;
	int bufi = 0;
	FILE * stream;
	stream = fopen("out.txt","ab+");
	while(cin >> bufc){
		bufi = ych(bufc)*16;
		cin >> bufc;
		bufi = bufi + ych(bufc);
		fputc(bufi,stream);
	}
	fclose(stream);
	return 0;
}

