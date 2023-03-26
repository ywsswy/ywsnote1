#include<iostream>
#include<string>
#include<vector>
#include<io.h>
#include <opencv2/opencv.hpp>
#include<sstream>
using namespace std;
using namespace cv;
void on_Trackbar1(int par, void *pvoid)
{
	static void *p0 = (void*)(*(0 + ((int*)pvoid)));
	static void *p1 = (void*)(*(1 + ((int*)pvoid)));
	static void *p2 = (void*)(*(2 + ((int*)pvoid)));
	static void *p3 = (void*)(*(3 + ((int*)pvoid)));
	static void *p4 = (void*)(*(4 + ((int*)pvoid)));
	string *win = (string*)p0;
	cv::Mat *mgray = (cv::Mat*)p1;
	cv::Mat *mbin = (cv::Mat*)p2;
	int *bs = (int*)p3;
	int *value = (int*)p4;
	*bs = par;//diff
	adaptiveThreshold(*mgray, *mbin, 255,
		cv::ADAPTIVE_THRESH_MEAN_C, cv::THRESH_BINARY,
		*bs = (*bs > 1 ? ((*bs) % 2 == 1 ? *bs : *bs + 1) : 3),
		*value);
	imshow(*win,*mbin);
}
int main(){
	Mat img = imread("C:\\Users\\Administrator\\Desktop\\黑客.jpg");
	cvtColor(img,img, CV_BGR2GRAY);
	Mat mbin;
	string s1 = "win1";
	int bs = 11;
	int value = 11;
	int *p[5];
	p[0] = (int*)&s1;
	p[1] = (int*)&img;
	p[2] = (int*)&mbin;
	p[3] = (int*)&bs;
	p[4] = (int*)&value;
	cv::createTrackbar("bar1", s1, &value, 255, on_Trackbar1,p);//单个滚动条，可以不指明用户参数，通过下面的显示调用来指明
	on_Trackbar1(value, p);
	waitKey();
	return 0;
}

