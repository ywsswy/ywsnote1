#define YOK 1
#define YERROR 0
#define YStatus int
#include "highgui.h"
/*flag=1,char[];*/
void yGetPar1(int flag,char s[]){
	if (flag == 1)
		gets(s);
}
YStatus yShoPic(IplImage *img, char s[]){
	if (!(img = cvLoadImage(s))){
		return YERROR;
	}
	else
	{
		cvNamedWindow("CNW1", CV_WINDOW_AUTOSIZE);
		cvShowImage("CNW1", img);
		cvWaitKey(0);
		cvReleaseImage(&img);
		cvDestroyWindow("CNW1");
		return YOK;
	}
}
int main(
#if 0 // import 2, argv[0]="MyOpenCV.exe",argv[1]="mr.jpg"
	int argc, char** argv
#endif
	){
#if 0
	IplImage*img = cvLoadImage(argv[1]);
#endif
#if 0
	IplImage *img = cvLoadImage("D:\\program files (x86)\\opencv\\projects\\MyOpenCV\\Debug\\mr.jpg");
	//D:\program files (x86)\opencv\projects\MyOpenCV\Debug\mr.jpg
#endif
#if 1
	IplImage *img = NULL;
	char s1[400] = "";
#endif
	yGetPar1(1,s1);
	yShoPic(img, s1);
	return 0;
}
