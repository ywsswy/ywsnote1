#include "cv.h"
#include "highgui.h"
using namespace cv;
int main(){
	Mat i1 = imread("F:\\fc\\document\\tb\\identity\\2-1.jpg",1);
	cvtColor(i1, i1, CV_BGR2GRAY);
	i1 = 255 - i1;
	return 0;
}
