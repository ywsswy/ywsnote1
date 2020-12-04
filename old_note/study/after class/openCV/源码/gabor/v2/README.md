///////////////////////
#include"cvgabor.h"
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <iostream>
#include <vector>
static int nThreshold1 = PI;
static int nThreshold2 = 5;
//创建一个方向是PI/4而尺度是3的gabor
double Sigma = 2 * PI;
double F = sqrt(2.0);
Mat img = imread("test.jpg", CV_LOAD_IMAGE_GRAYSCALE);
void on_trackbar(int pos){
	CvGabor gabor(PI, nThreshold2, Sigma, F);
	//获得实部并显示它
	Mat kernel(gabor.get_mask_width(), gabor.get_mask_width(), CV_8UC1);
	gabor.get_image(CV_GABOR_REAL, kernel);
	imshow("Kernel", kernel);
	cout << "row:" << kernel.rows << ",cols" << kernel.cols << endl;
	//获取载入图像的gabor滤波响应的实部并且显示
	Mat reimg(img.rows, img.cols, CV_32FC1);
	gabor.conv_img(img, reimg, CV_GABOR_REAL);
	imshow("After Image", reimg);
}
int main(){
	cvNamedWindow("Test", 1);
	imshow("Original Image", img);
	cvCreateTrackbar("dPhi", "Test", &nThreshold1, 7, on_trackbar);
	cvCreateTrackbar("iNu", "Test", &nThreshold2, 4, on_trackbar);
	on_trackbar(NULL);//初始显示阈值
	cvWaitKey(0);
	cvDestroyWindow("Test");
}
