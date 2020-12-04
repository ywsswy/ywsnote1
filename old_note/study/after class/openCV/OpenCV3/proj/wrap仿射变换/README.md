#include<opencv2/opencv.hpp>  
#include<iostream>  
#include<vector>  
  
using namespace cv;  
using namespace std;  
  
int main()  
{  
    Mat srcImage = imread("tiger.jpg");  
    imshow("【原图】", srcImage);  
  
    Mat dstImage;  
    dstImage.create(srcImage.size(), srcImage.type());  
    Mat MArray(2, 3, CV_32FC1);  
  
    ////输入源三角形的三个点的坐标，和输出三角形的三个点的坐标，建立对应关系  
    //Point2f srcPoint2f[3], dstPoint2f[3];  
    //srcPoint2f[0] = Point2f(static_cast<float>(0), static_cast<float>(0));  
    //srcPoint2f[1] = Point2f(static_cast<float>(srcImage.cols - 1), static_cast<float>(0));  
    //srcPoint2f[2] = Point2f(static_cast<float>(0), static_cast<float>(srcImage.rows - 1));  
  
    //dstPoint2f[0] = Point2f(static_cast<float>(0), static_cast<float>(srcImage.rows * 0.33));  
    //dstPoint2f[1] = Point2f(static_cast<float>(srcImage.cols * 0.65), static_cast<float>(srcImage.rows * 0.35));  
    //dstPoint2f[2] = Point2f(static_cast<float>(srcImage.cols * 0.15), static_cast<float>(srcImage.rows * 0.6));  
  
    //输入源三角形的三个点的坐标，和输出三角形的三个点的坐标，建立对应关系  
    Point2f srcPoint2f[3], dstPoint2f[3];  
    srcPoint2f[0] = Point2f(static_cast<float>(0), static_cast<float>(0));  
    srcPoint2f[1] = Point2f(static_cast<float>(0), static_cast<float>(3));  
    srcPoint2f[2] = Point2f(static_cast<float>(3), static_cast<float>(0));  
  
    dstPoint2f[0] = Point2f(static_cast<float>(0), static_cast<float>(3));  
    dstPoint2f[1] = Point2f(static_cast<float>(3), static_cast<float>(5));  
    dstPoint2f[2] = Point2f(static_cast<float>(5), static_cast<float>(3));  
  
    //用getAffineTransform函数建立对应关系  
    MArray = getAffineTransform(srcPoint2f, dstPoint2f);  
  
    warpAffine(srcImage, dstImage, MArray, Size(srcImage.cols * 3, srcImage.rows * 3), 1, 0, Scalar(0));  
  
    namedWindow("【进行仿射变换后】", 0);  
    imshow("【进行仿射变换后】", dstImage);  
  
    waitKey(0);  
  
    return 0;  
}  
