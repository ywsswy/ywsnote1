constructor initializer lists are only allowed on constructor definitions
:age(a)只有构造函数能用的写法，普通函数老老实实在函数体里赋值吧
【错】
CStudent::CStudent(char *n, int a):age(a){
	int nLen = strlen(n);
	name = new char[nLen+1];
	strcpy(name,n);
	name[nLen] = '\0';
}
void CStudent::setAge(int a):age(a){};
CStudent::~CStudent(){
	delete[] name;
}
