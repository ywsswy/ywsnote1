//////////////////////////
#include <opencv2/opencv.hpp>
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <iostream>
#include <vector>
const double PI = 3.14159265;
using namespace cv;
using namespace std;
static int nThreshold1 = 62;
static int nThreshold2 = 3;
static int nThreshold3 = 0;
static int nThreshold4 = 2;
static int nThreshold5 = 2;
Mat img;
Mat Kern, Out;
CvSize s1;
void on_trackbar(int pos){
	double thta = 0.01 * PI * nThreshold3;
	double gama = 0.1 * nThreshold5;
	s1 = Size(nThreshold1, nThreshold1);
	Kern = getGaborKernel(s1, 1.*nThreshold2, thta, 1.*nThreshold4, gama);
	imshow("kern", Kern);
	filter2D(img, Out, -1, Kern);
	imshow("out", Out);
}
int main(){
	cvNamedWindow("Test", 1);
	//kernelsize 接受区域的大小σ 方向（0为垂直) 波长（像素/宽度) 纵横比（伸展长度）
	img = imread("timg2.jpg", CV_LOAD_IMAGE_GRAYSCALE);
	imshow("Original Image", img);
	cvCreateTrackbar("size", "Test", &nThreshold1, 100, on_trackbar);
	cvCreateTrackbar("σ", "Test", &nThreshold2, 40, on_trackbar);
	cvCreateTrackbar("θ（π%）", "Test", &nThreshold3, 100, on_trackbar);
	cvCreateTrackbar("λ", "Test", &nThreshold4, 10, on_trackbar);
	cvCreateTrackbar("0.1*γ", "Test", &nThreshold5, 40, on_trackbar);
	on_trackbar(NULL);
	cvWaitKey(0);
	cvDestroyWindow("Test");
}
