#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <vector>
#include <stdio.h>  
#include <cv.h>  
#include <cxcore.h>  
#include <highgui.h>  
#include <opencv2/opencv.hpp>
#include <iostream>
using namespace cv;
void rotate_test()
{
}
static int n1 = 0;
static int n2 = 1;
static int n3 = 0;
static int n4 = 0;
Mat		image(240, 320, CV_8U, Scalar(0));
void on_trackbar(int pos){
	Point   center(n3,n4);
	Mat R = getRotationMatrix2D(center, float(n1), (float)n2);
	Mat imgR;
	warpAffine(image, imgR, R, Size(320, 240));
	imshow("Rotate image", imgR);
}
int main(){
	cvNamedWindow("age", 1);
	rectangle(image, Rect(80, 60, 100, 50), Scalar(255), CV_FILLED);
	imshow("Image", image);
	cvCreateTrackbar("angle", "age", &n1, 360, on_trackbar);
	cvCreateTrackbar("scale", "age", &n2, 4, on_trackbar);
	cvCreateTrackbar("x", "age", &n3, 320, on_trackbar);
	cvCreateTrackbar("y", "age", &n4, 240, on_trackbar);
	on_trackbar(NULL);
	cvWaitKey(0);
	cvDestroyWindow("age");
	return 0;
}
