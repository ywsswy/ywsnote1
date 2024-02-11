#include<opencv2/opencv.hpp>
#include<iostream>
using namespace std;
using namespace cv;
int main(){
#if 0
	FileStorage fs("test.yaml", FileStorage::WRITE);
//	FileStorage fs("test.yaml", FileStorage::APPEND);
	fs << "t1" << 5;//切记，FileNode不能以数字开头
	fs << "t2" << 3.5;
	fs.release();
#endif
	double f1 = 0;
	FileStorage fs2("test.yaml", FileStorage::READ);
	fs2["t2"] >> f1;
	fs2.release();
	cout << f1 << endl;
	return 0;
}
/*
XML，eXtensible Markup Language,可扩展标记语言，元标记语言。
YAML（.yml or .yaml）
#为注释
*/
