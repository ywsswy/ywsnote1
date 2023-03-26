#include <stdio.h>
#include <iostream>
#include "opencv2/core/core.hpp"
#include "opencv2/features2d/features2d.hpp"
#include "opencv2/highgui/highgui.hpp"
#include "opencv2/nonfree/features2d.hpp"
#include <opencv2/opencv.hpp>
#include<time.h>
using namespace cv;
using namespace std;
Mat src1, src2, dst;
CvMat* warp_mat = cvCreateMat(2, 3, CV_32FC1);
std::vector<KeyPoint> good_keypoints_1, good_keypoints_2;
std::vector< DMatch > good_matches;
CvScalar s;
int width, height;
std::vector<Point2f> frame1_features_ok, frame2_features_ok;
Point2f tmpPoint;
void getMatcher(void){
	int a = clock();
	int b = 0;
	//-- Step 1: Detect the keypoints using SURF Detector
	int minHessian = 2000;//400~800
	SurfFeatureDetector detector(minHessian);
	std::vector<KeyPoint> keypoints_1, keypoints_2;
	detector.detect(src1, keypoints_1);
	detector.detect(src2, keypoints_2);
	int i, count = 0;
	b = clock();
	cout << b - a << endl;
	//-- Step 2: Calculate descriptors (feature vectors)
	SurfDescriptorExtractor extractor;
	Mat descriptors_1, descriptors_2;
	extractor.compute(src1, keypoints_1, descriptors_1);
	extractor.compute(src2, keypoints_2, descriptors_2);
	b = clock();
	cout <<"h"<< b - a << endl;
	//-- Step 3: Matching descriptor vectors with a brute force matcher
	FlannBasedMatcher matcher;
	std::vector< DMatch > matches;
	matcher.match(descriptors_1, descriptors_2, matches);
	b = clock();
	cout << b - a << endl;
	//-- Step 4: Find good matcher
	double mina=0.0, minb=0.0, minc=0.0;
	int min1 = -1, min2 = -1, min3 = -1, flag = -1;
	for (int i = 0; i < descriptors_1.rows; i++){
		if (descriptors_1.rows < 3){
			cout << "hh" << endl;
		}
		else{
			if (mina>minb&&mina> minc){
				flag = 1;
			}
			else if (minb > minc){
				flag = 2;
			}
			else{
				flag = 3;
			}
			double dist = matches[i].distance;
			if (min1 == -1){
				min1 = i; mina = dist;
			}
			else if (min2 == -1){
				min2 = i; minb = dist;
			}
			else if (min3 == -1){
				min3 = i; minc = dist;
			}
			else if (dist < mina&&flag == 1){
				min1 = i;
				mina = dist;
			}
			else if (dist < minb&&flag == 2){
				min2 = i;
				minb = dist;
			}
			else if (dist < minc&&flag == 3){
				min3 = i;
				minc = dist;
			}
		}
	}
	for (i = 0; i < descriptors_1.rows; i++){
		if (i==min1||i==min2||i==min3){
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
	b = clock();
	cout << b - a << endl;
	//-- Step 5:Show detected matches
	imshow("src1", src1);
	imshow("src2", src2);
	b = clock();
	cout << b - a << endl;
}
int cutSport(void){
	Mat mat;
	src1.copyTo(dst);
	mat = estimateRigidTransform(frame1_features_ok, frame2_features_ok, true);
	warpAffine(src1, dst, mat, Size(src1.cols, src1.rows));// , 1, 0, Scalar(0));//
	imshow("result", dst);
	return 0;
}
int main(
#if 0
	int argc, char** argv
#endif
	)
{
#if 1
	int argc = 3;
	char *argv[3] = { "", "Debug\\1.jpg", "Debug\\2.jpg" };
#endif
	int a = clock();
	int b = 0;
	if (argc != 3){
		printf("Please input pic1 and pic2!\n");
		return -1;
	}
	src1 = imread(argv[1], 0);
	src2 = imread(argv[2], 0);
	width = src1.cols;
	height = src1.rows;
	getMatcher();
	cutSport();
	b = clock();
	cout << b - a << endl;
	waitKey(0);
	return 0;
}

