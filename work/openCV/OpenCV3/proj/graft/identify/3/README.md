#include "cv.h"
#include "highgui.h"
#include <iostream>
using namespace cv;
using namespace std;
Mat gray;
Mat gray1;
Mat cgray1;
Mat bin;
Scalar color;
char str[100] = "";
char buf[100] = "";
int pos;
void on_trackbar(int, void*){
	vector<float> yarea;
	vector<float> ywidth;
	vector<float> yheight;
	vector<vector<Point> > contour;
	vector<Vec4i> hierarchy;
	threshold(cgray1, bin, pos, 255, THRESH_BINARY);
	imshow("cgray1", bin);
	findContours(bin, contour, hierarchy, RETR_CCOMP, CHAIN_APPROX_SIMPLE);
	for (int i = 0; i < contour.size(); i++){
		yarea.push_back(contourArea(contour[i], false));
		Rect r0 = boundingRect(contour[i]);
		ywidth.push_back(r0.width);
		yheight.push_back(r0.height);
	}
	Mat ycanvas = Mat::zeros(Size(bin.cols, bin.rows), CV_8UC3);
	color = Scalar(rand() & 255, rand() & 255, rand() & 255);
	drawContours(ycanvas, contour, -1, color, 1, 8, hierarchy);
	imshow("contours1", ycanvas);
	auto&& biggest = max_element(begin(yarea), end(yarea));
	int i = distance(begin(yarea), biggest);
	cout <<i<< "..."<<yarea[i] << "..." << ywidth[i] << "..." << yheight[i] << endl;
}
//	(202,477)	(548,540)
void rota(double angle){
	Mat R = getRotationMatrix2D(Point(300, 300), angle, 1.0);
	warpAffine(gray1, gray1, R, Size(gray1.rows, gray1.cols));
	cgray1 = gray1(Rect(Point(202, 477), Point(548, 540)));
	for (int i = 5; i <= 55; i = i + 10){
		for (int j = (i * 2 + 5) % 30; j < 320; j = j + 30){
			line(cgray1, Point(j, i), Point(j + 25, i), Scalar(255), 2);
		}
	}
	pos = 254;
	createTrackbar("阈值", "BAR", &pos, 254, on_trackbar);
	on_trackbar(0,0);
	waitKey(0);
}
int main(){
	gray = imread("F:\\fc\\document\\tb\\identity\\2-1.jpg",1);
	cvtColor(gray, gray, CV_BGR2GRAY);
	gray = 255 - gray;
	resize(gray, gray, Size(600, 600));
	namedWindow("gray", 1);
	namedWindow("cgray1", 1);
	namedWindow("contours1", 1);
	namedWindow("BAR", 1);
	imshow("gray", gray);
	gray1 = gray.clone();
	for (int an = 0; an < 4; an++){
		rota(90.0*an);
	}
	return 0;
}
