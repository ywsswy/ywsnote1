//////////////////////////////////////
#include"cvgabor.h"
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <iostream>
#include <vector>
int main(){
	//创建一个方向是PI/4而尺度是3的gabor
	double Sigma = 2 * PI;
	double F = sqrt(2.0);
	CvGabor gabor(PI*3/4, 3, Sigma, F);
	//获得实部并显示它
	Mat kernel(gabor.get_mask_width(), gabor.get_mask_width(), CV_8UC1);
	gabor.get_image(CV_GABOR_REAL, kernel);
	imshow("Kernel", kernel);
	cout << kernel.rows << endl;
	cout << kernel.cols << endl;
	cvWaitKey(0);
	//载入一个图像并显示
	Mat img = imread("test.jpg", CV_LOAD_IMAGE_GRAYSCALE);
	imshow("Original Image", img);
	//获取载入图像的gabor滤波响应的实部并且显示
	Mat reimg(img.rows, img.cols, CV_32FC1);
	gabor.conv_img(img, reimg, CV_GABOR_REAL);
	imshow("After Image", reimg);
	cvWaitKey(0);
}
