#include<opencv2/opencv.hpp>
#include<iostream>
#include<string>
using namespace std;
using namespace cv;
int main()
{
	string hh = R"(F:\fc\document\tb\recent\2017-05-11数字（电子）\2017-05-11\192.168.2.195_01_20170511172939466.jpg)";
	//"YS-201.jpg"
	Mat img = imread(hh);
	int img_height = img.rows;
	int img_width = img.cols;
	vector<Point2f> corners(4);
	corners[0] = Point2f(117,169);
	corners[1] = Point2f(117,285);
	corners[2] = Point2f(448,169);
	corners[3] = Point2f(448,285);
	vector<Point2f> srcTri(4);
	FILE*fp = NULL;
	fp = fopen("D:\\tmp\\hh.txt", "r");
	fscanf(fp, "%f%f", &srcTri[0].x, &srcTri[0].y);
	fscanf(fp, "%f%f", &srcTri[1].x, &srcTri[1].y);
	fscanf(fp, "%f%f", &srcTri[2].x, &srcTri[2].y);
	fscanf(fp, "%f%f", &srcTri[3].x, &srcTri[3].y);
	fclose(fp);
	Mat MArray = getPerspectiveTransform(srcTri, corners);
#if 1
	vector<Point2f> points, points_trans;
	for (int i = 0; i < img_height; i++){
		for (int j = 0; j < img_width; j++){
			points.push_back(Point2f(j, i));
		}
	}
	perspectiveTransform(points, points_trans, MArray);
	Mat img_trans = Mat::zeros(img_height, img_width, CV_8UC3);
	int count = 0;
	for (int i = 0; i < img_height; i++){
		uchar* p = img.ptr<uchar>(i);
		for (int j = 0; j < img_width; j++){
			int y = points_trans[count].y;
			int x = points_trans[count].x;
			count++;
			if (y < 0 || x < 0 || y + 1 >= img_height || x + 1 >= img_width)
				continue;
			uchar* t = img_trans.ptr<uchar>(y);
			t[x * 3] = p[j * 3];
			t[x * 3 + 1] = p[j * 3 + 1];
			t[x * 3 + 2] = p[j * 3 + 2];
		}
	}
	imwrite("D:\\tmp\\mm2.jpg", img_trans);
	imshow("hh", img_trans);
	waitKey();
#endif
	FileStorage fs("D:\\tmp\\hh2.yaml", FileStorage::WRITE);
	fs<<"p11"<<MArray.at<double>(0, 0);
	fs<<"p12"<<MArray.at<double>(0, 1);
	fs<<"p13"<<MArray.at<double>(0, 2);
	fs<<"p21"<<MArray.at<double>(1, 0);
	fs<<"p22"<<MArray.at<double>(1, 1);
	fs<<"p23"<<MArray.at<double>(1, 2);
	fs<<"p31"<<MArray.at<double>(2, 0);
	fs<<"p32"<<MArray.at<double>(2, 1);
	fs<<"p33"<<MArray.at<double>(2, 2);
	fs.release();
	/*/
	Mat img = imread("YS-100.jpg");
	int img_height = img.rows;
	int img_width = img.cols;
	vector<Point2f> corners(4);
	corners[0] = Point2f(0, 0);
	corners[1] = Point2f(0, img_height - 1);
	corners[2] = Point2f(img_width - 1, 0);
	corners[3] = Point2f(img_width - 1, img_height - 1);
	vector<Point2f> srcTri(4);
	FILE*fp = NULL;
	fp = fopen("D:\\tmp\\hh.txt", "r");
	fscanf(fp, "%f%f", &srcTri[0].x, &srcTri[0].y);
	fscanf(fp, "%f%f", &srcTri[1].x, &srcTri[1].y);
	fscanf(fp, "%f%f", &srcTri[2].x, &srcTri[2].y);
	fscanf(fp, "%f%f", &srcTri[3].x, &srcTri[3].y);
	fclose(fp);
	Mat transform = getPerspectiveTransform(corners, srcTri);
	vector<Point2f> points, points_trans;
	for (int i = 0; i < img_height; i++){
		for (int j = 0; j < img_width; j++){
			points.push_back(Point2f(j, i));
		}
	}
	perspectiveTransform(points, points_trans, transform);
	Mat img_trans = Mat::zeros(img_height, img_width, CV_8UC3);
	int count = 0;
	for (int i = 0; i < img_height; i++){
		uchar* p = img.ptr<uchar>(i);
		for (int j = 0; j < img_width; j++){
			int y = points_trans[count].y;
			int x = points_trans[count].x;
			uchar* t = img_trans.ptr<uchar>(y);
			t[x * 3] = p[j * 3];
			t[x * 3 + 1] = p[j * 3 + 1];
			t[x * 3 + 2] = p[j * 3 + 2];
			count++;
		}
	}
	imwrite("YS-201.jpg", img_trans);
	*/
	return 0;
}
