#include <opencv2/opencv.hpp>
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <iostream>
#include <vector>
#include<fstream> 
#include "GraphUtils.h"
const double PI = 3.14159265;
using namespace cv;
using namespace std;
static int nThreshold1 = 34;
static int nThreshold2 = 3;
static int nThreshold3 = 0;
static int nThreshold4 = 100;
static int nThreshold5 = 4;
float comp[362] = { 0 };
Mat img;
Mat img2;
Mat Kern, Out;
CvSize s1;
const char *pstrWindowsHistTitle = "直方图";
void yhist(){
	uchar *pt;      //创建指针
	double thta = 0;
	s1 = Size(nThreshold1, nThreshold1);
	for (nThreshold3 = 0; nThreshold3 < 360; nThreshold3++){
		comp[nThreshold3] = 0;
		thta = PI / 180.0 * nThreshold3;
		Kern = getGaborKernel(s1, 1.*nThreshold2, thta, 1.*nThreshold4, 1.*nThreshold5);
		filter2D(img, Out, -1, Kern);
		for (int row = 0; row < Out.rows; row++){
			pt = Out.ptr<uchar>(row);
			for (int col = 0; col < Out.cols; col++){
				int a = pt[col];
				//cout << "此像素点数值为" << a << endl;
				if (a){
					//cout << "统计中的数值" << counts[th] << endl;
					comp[nThreshold3] += a;
				}
			}
		}
		//cout << "角度为" << thta << "的响应值为" << comp[ << endl;
	}
}
int main(){
	img2 = imread("srcc.jpg", CV_LOAD_IMAGE_GRAYSCALE);
	imshow("Original Image", img2);
	resize(img2, img, Size(img2.cols / 2, img2.rows / 2));
	yhist();
	IplImage *bgImg = cvLoadImage("grid.jpg");
	drawFloatGraph(comp, 360, bgImg, 250000.0F, 450000.0F, bgImg->width, bgImg->height);
	showImage(bgImg);
	waitKey();
}
