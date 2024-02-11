///////////////////////////////
//////////////////////////////////////
#include"cvgabor.h"
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <iostream>
#include <vector>
//载入一个图像并显示
Mat img = imread("test.jpg", CV_LOAD_IMAGE_GRAYSCALE);
double Sigma = 2*PI;
double F = sqrt(2.0);
void f(double f1){
	CvGabor gabor(f1, 5, Sigma, F);
	//获得实部并显示它
	Mat kernel(gabor.get_mask_width(), gabor.get_mask_width(), CV_8UC1);
	gabor.get_image(CV_GABOR_REAL, kernel);
	//imshow("Kernel", kernel);
	//cout << "row:" << kernel.rows << ",cols" << kernel.cols << endl;
	//获取载入图像的gabor滤波响应的实部并且显示
	Mat reimg(img.rows, img.cols, CV_32FC1);
	gabor.conv_img(img, reimg, CV_GABOR_REAL);
	//imshow("After Image", reimg);
	Mat tmp_m, tmp_sd;
	double m = 0, sd = 0;
	meanStdDev(reimg, tmp_m, tmp_sd);
	m = tmp_m.at<double>(0, 0);
	sd = tmp_sd.at<double>(0, 0);
	cout << "th:" << f1 / 3.14 << ",mean:"<<m<<",StdDev: " << sd << endl;
	waitKey();
}
int main(){
	//C:\\Users\\admin\\Desktop\\tb\\ 
	imshow("Original Image", img);
	double yth = 0.0;
  	for (; yth < 3.14; yth = yth + 0.1){
  		f(yth);
  	}
	cvWaitKey(0);
}
