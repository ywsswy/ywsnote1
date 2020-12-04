//直接灰度图，完整版
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
Mat ycall(Mat src1, Mat src2){
	CvMat* warp_mat = cvCreateMat(2, 3, CV_32FC1);
	std::vector<KeyPoint> good_keypoints_1, good_keypoints_2;
	std::vector< DMatch > good_matches;
	CvScalar s;
	int width, height;
	std::vector<Point2f> frame1_features_ok, frame2_features_ok;
	Point2f tmpPoint;
	int a = clock();
	width = src1.cols;
	height = src1.rows;
	//getMatcher();
	//-- Step 1: Detect the keypoints using SURF Detector
	int minHessian = 2000;//400~800
	SurfFeatureDetector detector(minHessian);
	std::vector<KeyPoint> keypoints_1, keypoints_2;
	detector.detect(src1, keypoints_1);
	detector.detect(src2, keypoints_2);
	int i, count = 0;
	//-- Step 2: Calculate descriptors (feature vectors)
	SurfDescriptorExtractor extractor;
	Mat descriptors_1, descriptors_2;
	extractor.compute(src1, keypoints_1, descriptors_1);
	extractor.compute(src2, keypoints_2, descriptors_2);
	//-- Step 3: Matching descriptor vectors with a brute force matcher
	FlannBasedMatcher matcher;
	std::vector< DMatch > matches;
	matcher.match(descriptors_1, descriptors_2, matches);
	//-- Step 4: Find good matcher
	double max_dist = 0; double min_dist = 100;
	if (descriptors_1.rows < 4){
		printf("hh\n");
		exit(0);
	}
	for (i = 0; i < descriptors_1.rows; i++){
		double dist = matches[i].distance;
		if (dist < min_dist)
			min_dist = dist;
		if (dist > max_dist)
			max_dist = dist;
	}
	for (i = 0; i < descriptors_1.rows; i++){
		if (matches[i].distance < 2 * min_dist){
			count += 1;
			good_matches.push_back(matches[i]);
			good_keypoints_1.push_back(keypoints_1[matches[i].queryIdx]);
			good_keypoints_2.push_back(keypoints_2[matches[i].trainIdx]);
		}
	}
	for (i = 0; i < count; i++){
		circle(src1, cvPoint(good_keypoints_1[i].pt.x, good_keypoints_1[i].pt.y),
			3, cvScalar(255, 0, 0), 0);
		circle(src2, cvPoint(good_keypoints_2[i].pt.x, good_keypoints_2[i].pt.y),
			3, cvScalar(255, 0, 0), 0);
		tmpPoint.x = good_keypoints_1[i].pt.x;
		tmpPoint.y = good_keypoints_1[i].pt.y;
		frame1_features_ok.push_back(tmpPoint);
		tmpPoint.x = good_keypoints_2[i].pt.x;
		tmpPoint.y = good_keypoints_2[i].pt.y;
		frame2_features_ok.push_back(tmpPoint);
	}
	//-- Step 5:Show detected matches
	//imshow("src1", src1);
	//imshow("src2", src2);
	//
	Mat mat;
	Mat dst(src1.rows, src1.cols, CV_8UC1);
	mat = estimateRigidTransform(frame1_features_ok, frame2_features_ok, true);
	warpAffine(src1, dst, mat, Size(src1.cols, src1.rows));
	//imshow("result", dst);
	int b = clock();
	cout << b - a << endl;
	waitKey(0);
	return dst;
}
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
void cacheCount(Mat m1){
	uchar*pt;
	double sum = 0;
	for (int i = 0; i < m1.rows; i++){
		pt = m1.ptr<uchar>(i);
		for (int j = 0; j < m1.cols; j++){
			sum+=pt[j];
		}
	}
	cout << sum << endl;
}
void cvRegister2(int *dx, int *dy, Mat img4, Mat fimg4,int mul){
	int h = img4.rows;
	int w = img4.cols;
	int dx1 = *dx;
	int dy1 = *dy;
	Scalar avg;
	//cout << "w=" << w << ",h=" << h << endl;
	Mat imgA, imgB, imgC, imgD;
	Mat imgMAX, imgMIN, imgREA;
	//imgA:切割获取原始模版图片（1920*1080）中间1/3的感兴趣区域
	//imgB:扫描切割获取测试图片（1920*1080）的等大区域
	//imgC:imgA-imgB
	int mini, minj;
	double minvalue, value;
	double mean1, mean2;
	//保存在(mini行,minj列)处对齐最匹配的程度(最小差值和)minvalue;
	imgA = fimg4(Rect(w / 3, h / 3, w / 3, h / 3));
	imgMAX = imgA.clone();
	imgMIN = imgA.clone();
//	imshow("模版中心图", imgA);
	//cout << "w=" << imgA.cols << ",h=" << imgA.rows << endl;
	//cacheCount(imgA);
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = h/3+(*dy-1)*mul; i <= h/3+(*dy+1)*mul; i++){
		for (int j = w/3+(*dx-1)*mul; j <= w/3+(*dx+1)*mul; j++){
			value = 0;
			imgB = img4(Rect(j, i, w / 3, h / 3));
			//imshow("imgB", imgB);
 			imgC = imgB - imgA;
			avg = mean(imgC);
 			value = avg.val[0];
			imgC = imgA - imgB;
			avg = mean(imgC);
			value += avg.val[0];
			/*
			for (int ii = 0; ii < imgA.rows; ii++){
				for (int jj = 0; jj < imgA.cols; jj++){
					for (int k = 0; k<3; k++){
						if (imgA.at<cv::Vec3b>(ii, jj)[k]>imgB.at<cv::Vec3b>(ii, jj)[k]){
							imgMAX.at<cv::Vec3b>(ii, jj)[k] = imgA.at<cv::Vec3b>(ii, jj)[k];
							imgMIN.at<cv::Vec3b>(ii, jj)[k] = imgB.at<cv::Vec3b>(ii, jj)[k];
						}
						else{
							imgMAX.at<cv::Vec3b>(ii, jj)[k] = imgB.at<cv::Vec3b>(ii, jj)[k];
							imgMIN.at<cv::Vec3b>(ii, jj)[k] = imgA.at<cv::Vec3b>(ii, jj)[k];
						}
					}
					
				}
			}
			imgREA = imgMAX - imgMIN;
			avg = mean(imgREA);
			value = avg.val[0];
			*/
			//
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
	//cacheCount(imgB);
	if (1){
		dx1 = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
		dy1 = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	}
	else{
		*dx = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
		*dy = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	}
	//cout << "dx=" << *dx << ",dy=" << *dy << endl;
//  	imgD = imgB - imgA;
//  	imshow("相减图1", imgD);
// 	imgD = imgA - imgB;
//  	imshow("相减图2", imgD);
	avg = mean(imgA);
	mean1 = avg.val[0];
	avg = mean(imgB);
	mean2 = avg.val[0];
//  	cout << mean1 << endl;
//  	cout << mean2 << endl;
	///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	Mat cimg(1080, 1920, CV_8UC1, Scalar(static_cast<uchar>(fabs(mean2 - mean1))));
	if (mean1 < mean2){
		img4 = img4 - cimg;
	}
	else{
		img4 = img4 + cimg;
	}
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = h / 3 + (*dy - 1)*mul; i <= h / 3 + (*dy + 1)*mul; i++){
		for (int j = w / 3 + (*dx - 1)*mul; j <= w / 3 + (*dx + 1)*mul; j++){
			value = 0;
			imgB = img4(Rect(j, i, w / 3, h / 3));
			//imshow("imgB", imgB);
			imgC = imgB - imgA;
			avg = mean(imgC);
			value = avg.val[0];
			imgC = imgA - imgB;
			avg = mean(imgC);
			value += avg.val[0];
			/*
			for (int ii = 0; ii < imgA.rows; ii++){
			for (int jj = 0; jj < imgA.cols; jj++){
			for (int k = 0; k<3; k++){
			if (imgA.at<cv::Vec3b>(ii, jj)[k]>imgB.at<cv::Vec3b>(ii, jj)[k]){
			imgMAX.at<cv::Vec3b>(ii, jj)[k] = imgA.at<cv::Vec3b>(ii, jj)[k];
			imgMIN.at<cv::Vec3b>(ii, jj)[k] = imgB.at<cv::Vec3b>(ii, jj)[k];
			}
			else{
			imgMAX.at<cv::Vec3b>(ii, jj)[k] = imgB.at<cv::Vec3b>(ii, jj)[k];
			imgMIN.at<cv::Vec3b>(ii, jj)[k] = imgA.at<cv::Vec3b>(ii, jj)[k];
			}
			}
			}
			}
			imgREA = imgMAX - imgMIN;
			avg = mean(imgREA);
			value = avg.val[0];
			*/
			//
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
	//cacheCount(imgB);
	*dx = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
	*dy = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	//cout << "dx=" << *dx << ",dy=" << *dy << endl;
// 	imgD = imgB - imgA;
// 	imshow("相减图3", imgD);
// 	imgD = imgA - imgB;
// 	imshow("相减图4", imgD);
// 	avg = mean(imgA);
// 	mean1 = avg.val[0];
// 	avg = mean(imgB);
// 	mean2 = avg.val[0];
//  	cout << mean1 << endl;
//  	cout << mean2 << endl;
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
	realMin = 255.0 * 2 * 3 * w*h / 3 / 3;
	//保存在(mini行,minj列)处对齐最匹配的程度(最小差值和)minvalue;
	//模版图的中心1/3区域
	imgA = fimg4(Rect(w / 3, h / 3, w / 3, h / 3));
	//imshow("模版中心图", imgA);
	//cout << "w=" << imgA.cols << ",h=" << imgA.rows << endl;
#if 1
	//未考虑两张图大小不一
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = 0; i < h * 2 / 3; i++){
		for (int j = 0; j < w * 2 / 3; j++){
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
	}
	if (1){
		dx1 = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
		dy1 = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
	}
	imgB = img4(Rect(minj, mini, w / 3, h / 3));
	imshow("smallA", imgA);
	imshow("smallB", imgB);
	avg = mean(imgA);
	mean1 = avg.val[0];
	avg = mean(imgB);
	mean2 = avg.val[0];
// 	cout << mean1 << endl;
// 	cout << mean2 << endl;
	///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	Mat cimg(fimg4.rows , fimg4.cols, CV_8UC1, Scalar(static_cast<uchar>(fabs(mean2 - mean1))));
	if (mean1 < mean2){
		img4 = img4 - cimg;
	}
	else{
		img4 = img4 + cimg;
	}
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = 0; i < h * 2 / 3; i++){
		for (int j = 0; j < w * 2 / 3; j++){
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
#endif
#if 0
	//考虑到两张图大小不一
	
	//img4-resize
	//粗倍数；
	for (double ymul = 1.20; ymul > 1.00; ymul -= 0.05){
		resize(rimg4, img4, Size(ymul*w, ymul*h));
		minvalue = ymul*ymul*255.0 * 2 * 3 * w*h / 3 / 3;
		for (int i = 0; i < ymul* h * 2 / 3; i++){
			for (int j = 0; j < ymul* w * 2 / 3; j++){
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
		cout << "minvalue:" << minvalue << endl;
		if (minvalue < realMin){
			realMul = ymul;
			realMin = minvalue;
			*dx = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
			*dy = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
		}
	}
	//img4-resize
	imgA = fimg4(Rect(w / 3, h / 3, 0.8*w / 3, 0.8*h / 3));
	for (double ymul = 1.00; ymul > 0.80; ymul -= 0.05){
		resize(rimg4, img4, Size(ymul*w, ymul*h));
		minvalue = ymul*ymul*255.0 * 2 * 3 * w*h / 3 / 3;
		for (int i = 0; i < ymul* h * 2 / 3; i++){
			for (int j = 0; j < ymul* w * 2 / 3; j++){
				value = 0;
				imgB = img4(Rect(j, i, 0.8*w / 3, 0.8*h / 3));
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
		imgB = img4(Rect(minj, mini, 0.8*w / 3, 0.8*h / 3));
		//imshow("对齐中心图(small)", imgB);
		cout << "minvalue:" << minvalue << endl;
		if (minvalue < realMin){
			realMul = ymul;
			realMin = minvalue;
			*dx = minj - w / 3;//dx小于零时，测试图片 是 模版图片 往左移动得来
			*dy = mini - h / 3;//dy小于零时，测试图片 是 模版图片 往上移动得来
		}
	}
#endif
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
	//imshow("测试图", rimg4);
	//imshow("模版图", rfimg4);
	//realMul = cvRegister(dx,dy,rimg4,rfimg4,realMul);
	cvRegister(dx, dy, rimg4, rfimg4, realMul);
	//cout << realMul << endl;
	//exit(0);
	cvRegister2(dx,dy,img4,fimg4,mul);
	return;
}
int main(){
#if 0
	char xfimg4[100] = "E1-3.jpg";//原始模版图片路径
	char ximg4[100] = "2-3.jpg";//原始测试图片路径
#endif
#if 1
	char xfimg4[100] = "1\\f1.jpg";//原始模版图片路径
	char ximg4[100] = "1\\f2.jpg";//原始测试图片路径
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
	//img4 = ycall(img4, fimg4);
	//imshow("!!4", img4);
	//waitKey();
	int dx, dy;
	//gif(img4, fimg4);
	cvRegist(img4, fimg4,&dx,&dy,-1);
	cout << "dx=" << dx << ",dy=" << dy << endl;
	waitKey();
}
