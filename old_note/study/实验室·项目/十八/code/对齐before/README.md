#include <opencv2\opencv.hpp>
#include <opencv2\imgproc\imgproc.hpp>
#include <opencv2\highgui\highgui.hpp>
#include <opencv\cv.h>
#include <opencv\cv.hpp>
#include <stdio.h>
#include <iostream>
#include "opencv2/core/core.hpp"
#include "opencv2/features2d/features2d.hpp"
#include "opencv2/nonfree/features2d.hpp"
#include<time.h>
#include "opencv2/nonfree/features2d.hpp"
#include<time.h>
using namespace std;
using namespace cv;
void gif(Mat i1, Mat i2){
	char k = ' ';
	int flag = 0;
	while (k != 'q'){
		if (flag == 0){
			imshow("对齐效果图", i1);
		}
		else{
			imshow("对齐效果图", i2);
		}
		flag = !flag;
		k=waitKey(400);
	}
}
void cvRegister2(int *dx, int *dy, Mat img4, Mat fimg4,int mul){
	int h = img4.rows;
	int w = img4.cols;
	int dx1 = *dx;
	int dy1 = *dy;
	Scalar avg;
	//cout << "w=" << w << ",h=" << h << endl;
	Mat imgA, imgB, imgC, imgD;
	//imgA:切割获取原始模版图片（1920*1080）中间1/3的感兴趣区域
	//imgB:扫描切割获取测试图片（1920*1080）的等大区域
	//imgC:imgA-imgB
	int mini, minj;
	double minvalue, value;
	double mean1, mean2;
	//保存在(mini行,minj列)处对齐最匹配的程度(最小差值和)minvalue;
	imgA = fimg4(Rect(w / 3, h / 3, w / 3, h / 3));
//	imshow("模版中心图", imgA);
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = h/3+(*dy-1)*mul; i <= h/3+(*dy+1)*mul; i++){
		for (int j = w/3+(*dx-1)*mul; j <= w/3+(*dx+1)*mul; j++){
			if (i<0 || j<0 || (h / 3 + i)>h || (w / 3 + j)>w){
				continue;
			}
			value = 0;
			imgB = img4(Rect(j, i, w / 3, h / 3));
			//imshow("imgB", imgB);
 			imgC = imgB - imgA;
			avg = mean(imgC);
 			value = avg.val[0];
			imgC = imgA - imgB;
			avg = mean(imgC);
			value += avg.val[0];
			if (value < minvalue){
				mini = i;
				minj = j;
				minvalue = value;
			}
		}
	}
	//cout << "mini=" << mini << ",minj=" << minj << endl;
	//imgB = img4(Rect(minj, mini, w / 3, h / 3));
	//imshow("对齐中心图", imgB);
	if (1){
		dx1 = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
		dy1 = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	}
	else{
		*dx = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
		*dy = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	}
	//cout << "dx=" << *dx << ",dy=" << *dy << endl;
	avg = mean(imgA);
	mean1 = avg.val[0];
	avg = mean(imgB);
	mean2 = avg.val[0];
//  	cout << mean1 << endl;
//  	cout << mean2 << endl;
	///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	Mat cimg(img4.rows, img4.cols, CV_8UC1, Scalar(static_cast<uchar>(fabs(mean2 - mean1))));
	if (mean1 < mean2){
		img4 = img4 - cimg;
	}
	else{
		img4 = img4 + cimg;
	}
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = h / 3 + (*dy - 1)*mul; i <= h / 3 + (*dy + 1)*mul; i++){
		for (int j = w / 3 + (*dx - 1)*mul; j <= w / 3 + (*dx + 1)*mul; j++){
			if (i<0 || j<0 || (h / 3 + i)>h || (w / 3 + j)>w){
				continue;
			}
			value = 0;
			imgB = img4(Rect(j, i, w / 3, h / 3));
			//imshow("imgB", imgB);
			imgC = imgB - imgA;
			avg = mean(imgC);
			value = avg.val[0];
			imgC = imgA - imgB;
			avg = mean(imgC);
			value += avg.val[0];
			if (value < minvalue){
				mini = i;
				minj = j;
				minvalue = value;
			}
		}
	}
	//cout << "mini=" << mini << ",minj=" << minj << endl;
	imgB = img4(Rect(minj, mini, w / 3, h / 3));
//	imshow("对齐中心图2", imgB);
	*dx = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
	*dy = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	//cout << "dx=" << *dx << ",dy=" << *dy << endl;
	imgD = imgB - imgA;
	imshow("相减图3", imgD);
	imgD = imgA - imgB;
	imshow("相减图4", imgD);
	avg = mean(imgA);
	mean1 = avg.val[0];
	avg = mean(imgB);
	mean2 = avg.val[0];
  	cout << mean1 << endl;
  	cout << mean2 << endl;
	gif(imgA, imgB);
}
double cvRegister(int *dx, int*dy, Mat img4, Mat fimg4,double realMul){
	int h = fimg4.rows;
	int w = fimg4.cols;
	int dx1 = *dx;
	int dy1 = *dy;
	double mean1, mean2;
	Scalar avg;
	//cout << "w=" << w << ",h=" << h << endl;
	Mat imgA, imgB, imgC;
	//imgA:切割获取缩小后模版图片中间1/3的感兴趣区域
	//imgB:扫描切割获取缩小后测试图片的等大区域
	//imgC:imgA-imgB
	int mini, minj;
	double minvalue, value, realMin;
	
	//防止两张图的亮度差异（注：若偏移过大，是否不准？）
	avg = mean(fimg4);
	mean1 = avg.val[0];
	avg = mean(img4);
	mean2 = avg.val[0];
// 	cout << mean1 << endl;
//	cout << mean2 << endl;
	///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	Mat ccimg(fimg4.rows, fimg4.cols, CV_8UC1, Scalar(static_cast<uchar>(fabs(mean2 - mean1))));
	if (mean1 < mean2){
		img4 = img4 - ccimg;
	}
	else{
		img4 = img4 + ccimg;
	}
	realMin = 255.0 * 2 * 3 * w*h / 3 / 3;
	//保存在(mini行,minj列)处对齐最匹配的程度(最小差值和)minvalue;
	//模版图的中心1/3区域
	imgA = fimg4(Rect(w / 3, h / 3, w / 3, h / 3));
//	imshow("模版中心图", imgA);
	//cout << "w=" << imgA.cols << ",h=" << imgA.rows << endl;
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = 0; i < h * 2 / 3-2; i++){//change0326
		for (int j = 0; j < w * 2 / 3-2; j++){//change0326
			value = 0;
			imgB = img4(Rect(j, i, w / 3, h / 3));
			//imshow("imgB", imgB);
			imgC = imgB - imgA;
			//imshow("imgC", imgC);
			avg = mean(imgC);
			value = avg.val[0];
			imgC = imgA - imgB;
			avg = mean(imgC);
			value += avg.val[0];
			if (value < minvalue){
				mini = i;
				minj = j;
				minvalue = value;
			}
// 			if (i == h / 3 && j == w / 3){
// 				imgB = img4(Rect(j, i, w / 3, h / 3));
// 				avg = mean(imgB);
// 				mean1 = avg.val[0];
// 			}
		}
	}
	if (1){
		dx1 = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
		dy1 = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	}
	imgB = img4(Rect(minj, mini, w / 3, h / 3));
// 	imshow("smallA", imgA);
 //	imshow("smallB", imgB);
	avg = mean(imgA);
	mean1 = avg.val[0];
	avg = mean(imgB);
	mean2 = avg.val[0];
//  	cout << mean1 << endl;
//  	cout << mean2 << endl;
	///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	Mat cimg(fimg4.rows , fimg4.cols, CV_8UC1, Scalar(static_cast<uchar>(fabs(mean2 - mean1))));
	if (mean1 < mean2){
		img4 = img4 - cimg;
	}
	else{
		img4 = img4 + cimg;
	}
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = 0; i < h * 2 / 3-2; i++){//change0326
		for (int j = 0; j < w * 2 / 3-2; j++){//change0326
			value = 0;
			imgB = img4(Rect(j, i, w / 3, h / 3));
			//imshow("imgB", imgB);
			imgC = imgB - imgA;
			//imshow("imgC", imgC);
			avg = mean(imgC);
			value = avg.val[0];
			imgC = imgA - imgB;
			avg = mean(imgC);
			value += avg.val[0];
			if (value < minvalue){
				mini = i;
				minj = j;
				minvalue = value;
			}
		}
		//imgB = img4(Rect(w*2*1.15/3, i, w / 3, h / 3));
	}
	//cout  << "mini=" << mini << ",minj=" << minj << endl;
	imgB = img4(Rect(minj, mini, w / 3, h / 3));
	//imshow("对齐中心图(small)", imgB);
	//cout << "minvalue:" << minvalue << endl;
	//cout << "dx=" << *dx << ",dy=" << *dy << endl;
	*dx = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
	*dy = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	return realMul;
}
//测试图 模版图
//返回偏移dx dx小于零时，测试图片 是 模版图片 往左移动得来
//返回偏移dy dy小于零时，测试图片 是 模版图片 往上移动得来
//model -1：普通灰度化
//	0：HSV中H设为0
//	1：HSV中S设为0
//	2：HSV中V设为0 <==> 全黑 -- 不允许?
//未完
void cvRegist(Mat img4rgb,Mat fimg4rgb,int *dx,int*dy,int model = -1){
	Mat img4, fimg4;//测试图片与原始模版
	
	if (model == -1){
		cvtColor(img4rgb, img4, CV_BGR2GRAY);
		cvtColor(fimg4rgb, fimg4, CV_BGR2GRAY);
	}
	else{
		if (model >= 0 && model <= 2){
			cvtColor(img4rgb, img4, CV_BGR2HSV);
			cvtColor(fimg4rgb, fimg4, CV_BGR2HSV);
			for (int i = 0; i < img4.rows; i++){
				for (int j = 0; j < img4.cols; j++){
					img4.at<cv::Vec3b>(i, j)[model] = 255;
				}
			}
			for (int i = 0; i < fimg4.rows; i++){
				for (int j = 0; j < fimg4.cols; j++){
					fimg4.at<cv::Vec3b>(i, j)[model] = 255;
				}
			}
		}
		cvtColor(img4, img4, CV_HSV2BGR);
		cvtColor(fimg4, fimg4, CV_HSV2BGR);
		cvtColor(img4, img4, CV_BGR2GRAY);
		cvtColor(fimg4, fimg4, CV_BGR2GRAY);
	}
	
	int mul = 6;
	double realMul = 1.0;
	Mat rimg4, rfimg4;
	resize(img4, rimg4, Size(img4.cols / mul, img4.rows / mul));
	resize(fimg4, rfimg4, Size(fimg4.cols / mul, fimg4.rows / mul));
	imshow("测试图", rimg4);
	imshow("模版图", rfimg4);
	//realMul = cvRegister(dx,dy,rimg4,rfimg4,realMul);
	cvRegister(dx, dy, rimg4, rfimg4, realMul);
	//cout << realMul << endl;
	//exit(0);
	cvRegister2(dx,dy,img4,fimg4,mul);
	return;
}
int main(){
#if 0
	char xfimg4[100] = "T.JPG";//原始模版图片路径
	char ximg4[100] = "test0.jpg";//原始测试图片路径
#endif
#if 1
	char xfimg4[100] = "mod11.jpg";//原始模版图片路径
	char ximg4[100] = "mm2.jpg";//原始测试图片路径
#endif
#if 0
	char xfimg4[100] = "E1-3.jpg";//原始模版图片路径
	char ximg4[100] = "2-3.jpg";//原始测试图片路径
#endif
#if 0
	char xfimg4[100] = "1\\f1.jpg";//原始模版图片路径
	char ximg4[100] = "1\\f2.jpg";//原始测试图片路径
#endif
#if 0
	char xfimg4[100] = "1\\f3.jpg";//原始模版图片路径
	char ximg4[100] = "1\\f4.jpg";//原始测试图片路径
#endif
#if 0
	char xfimg4[100] = "35-1-1.jpg";//原始模版图片路径
	char ximg4[100] = "35-1-2.jpg";//原始测试图片路径
#endif
	//char xfimg4[100] = "fimg4.jpg";//原始模版图片路径
	//char ximg4[100] = "img4.jpg";//原始测试图片路径
	Mat img4, fimg4;
	img4 = imread(ximg4);
	fimg4 = imread(xfimg4);
	int dx, dy;
	cvRegist(img4, fimg4, &dx, &dy, -1);
	cout << "dx=" << dx << ",dy=" << dy << endl;
	waitKey();
}
