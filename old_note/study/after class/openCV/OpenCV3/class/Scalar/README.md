//////////////////////////////////////////////////////////////////////
默认四维向量
#include "opencv2/opencv.hpp"
using namespace cv;
int main(){
	Mat img(1,2, CV_8UC3, Scalar(1,5, 12));
	uchar *pt;
	pt = img.ptr<uchar>(0);
	cv::Scalar i = mean(img);
	double k = i.val[0];
	double j = i.val[1];
}
//获取三个通道各自的平均值

