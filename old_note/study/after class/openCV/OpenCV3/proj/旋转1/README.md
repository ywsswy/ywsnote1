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
	Mat		image(240, 320, CV_8U, Scalar(0));
	Point   center(130, 55);
	double  angle = 15;
	rectangle(image, Rect(80, 60, 100, 50), Scalar(255), CV_FILLED);
	Mat R = getRotationMatrix2D(center, angle, 1.0);// 逆时针旋转angle  
	Mat imgR;
	warpAffine(image, imgR, R, Size(320, 240));
	imshow("Image", image);
	imshow("Rotate image", imgR);
	waitKey(0);
}
int main(){
	rotate_test();
	return 0;
}
