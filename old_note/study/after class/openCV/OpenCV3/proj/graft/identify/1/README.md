#include "cv.h"
#include "highgui.h"
#include <iostream>
using namespace cv;
using namespace std;
Mat gray;
Mat bin;
Scalar color;
char str[100] = "";
char buf[100] = "";
CvFont font;
int pos;
CvMemStorage* storage = cvCreateMemStorage(0);
static void on_trackbar(int, void*){
	//vector<float> yarea;
	vector<float> ywidth;
	vector<float> yheight;
	vector<float> yy;
	vector<float> yx;
	vector<vector<Point> > contour;
	vector<Vec4i> hierarchy;
	threshold(gray, bin, pos, 255, THRESH_BINARY);
	imshow("Bin", bin);
	findContours(bin, contour, hierarchy, RETR_CCOMP, CHAIN_APPROX_SIMPLE);
	for (int i = 0; i < contour.size(); i++){
		//yarea.push_back(contourArea(contour[i], false));
		Rect r0 = boundingRect(contour[i]);
		ywidth.push_back(r0.width);
		yheight.push_back(r0.height);
		yy.push_back(r0.y);
		yx.push_back(r0.x);
	}
	Mat ycanvas = Mat::zeros(Size(bin.cols,bin.cols), CV_8UC3);
	color = Scalar(rand() & 255, rand() & 255, rand() & 255);
	drawContours(ycanvas, contour, -1, color,1, 8,hierarchy);
// 	string ys1 = "hhh";
	// 	putText(ycanvas, ys1,Point(0,100), FONT_HERSHEY_COMPLEX, 1, Scalar(0,0,255));//point 为左下角坐标
	for (int i = 0; i < contour.size(); i++){
		rectangle(ycanvas, Rect(yx[i], yy[i], ywidth[i], yheight[i]), color);
	}
	imshow("contours", ycanvas);
}
int main(){
	gray = imread("F:\\fc\\document\\tb\\identity\\2-1.jpg",1);
	cvtColor(gray, gray, CV_BGR2GRAY);
	gray = 255 - gray;
	resize(gray, gray, Size(600, 600));
	namedWindow("gray", 1);
	namedWindow("Bin", 1);
	namedWindow("contours", 1);
	namedWindow("BAR", 1);
	imshow("gray",gray);
	waitKey();
	srand(time(NULL));
	pos = 98;//初始滚动条位置，
	createTrackbar("阈值", "BAR", &pos, 254, on_trackbar);//254为滚动条最大值
	on_trackbar(0,0);//初始显示阈值
	waitKey(0);
	return 0;
}
