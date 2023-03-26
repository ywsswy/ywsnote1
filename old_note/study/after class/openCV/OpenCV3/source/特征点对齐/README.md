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
	//-- Step 1: Detect the keypoints using SURF Detector
	int minHessian = 2000;
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
	for (int i = 0; i < descriptors_1.rows; i++){
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
	imshow("src1", src1);
	imshow("src2", src2);
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
	int b = clock();
	cout << b - a << endl;
	waitKey(0);
	return 0;
}

