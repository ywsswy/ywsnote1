#define uchar unsigned char
typedef unsigned int uint;
//typedef ulong unsigned long;//error
typedef char str[81]; 
typedef char* pstr;
int iaa(int i){
	return i+1;
}
typedef int (*pfun)(int);
int main(){
	pfun p1;
	p1 = &iaa;
	return 0;
}
