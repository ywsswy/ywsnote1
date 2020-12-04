#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <vector>
#include <stdio.h>  
#include <cv.h>  
#include <cxcore.h>  
#include <highgui.h>  
#include <opencv2/opencv.hpp>
#include <iostream>
#include <time.h>
#include <windows.h>
using namespace std;
const double PI = 3.14159265;
//#pragma comment(linker, "/subsystem:\"windows\" /entry:\"mainCRTStartup\"")
IplImage* src;
IplImage* bin;
IplImage* gray;
IplImage* gray2;
IplImage* dst;
CvMemStorage* storage = cvCreateMemStorage(0);
CvSeq* contour = 0;
CvScalar color;
char str[100] = "";
char buf[100] = "";
CvFont font;
int bar = 255;
int flag = 0;
double maxarea = 0;
double maxvalue = 0;
CvPoint c1 = cvPoint(244,244);
double r1 = 110.0;//////////////////////////////////////////
double thta = 0.0;
double maxthta = 0;
void getBestTreshold(){
	//
	for (bar = 255; bar > 0 && flag == 0; bar--){
		cvThreshold(gray, bin, bar, 255, CV_THRESH_BINARY);
		cvShowImage("Bin", bin);
		cvFindContours(bin, storage, &contour, sizeof(CvContour), CV_RETR_CCOMP, CV_CHAIN_APPROX_SIMPLE);//提取轮廓 
		cvZero(dst);//清空数组     
		color = CV_RGB(rand() & 255, rand() & 255, rand() & 255);//创建一个色彩值 
		int count = 0;
		for (; contour != 0; contour = contour->h_next)
		{
			count++;
			double tmparea = fabs(cvContourArea(contour));//算面积
			CvRect aRect = cvBoundingRect(contour, 0);//算坐标与长宽
			//为消除过小连通域对画面产生的混乱感
			if (aRect.height < 105 || aRect.width < 105){
				cvSeqRemove(contour, 0); //删除某轮廓  
				continue;
			}
			if (aRect.height > 200 && aRect.width > 200 && tmparea > 20000 && aRect.height < 350){
				flag = 1;
				maxarea = tmparea;
			}
			color = CV_RGB(0, 255, 255);
			cvDrawContours(dst, contour, color, color, -1, 1, 8);//绘制外部和内部的轮廓 
			color = CV_RGB(rand() & 255, rand() & 255, rand() & 255);//创建一个色彩值
			strcpy(str, "C");
			itoa(count, buf, 10);
			strcat(str, buf);
			strcat(str, " :Area= ");
			itoa(static_cast<int>(tmparea), buf, 10);
			strcat(str, buf);
			strcat(str, ",Width= ");
			itoa(aRect.width, buf, 10);
			strcat(str, buf);
			strcat(str, ",Height= ");
			itoa(aRect.height, buf, 10);
			strcat(str, buf);
			strcat(str, ".");
			cvPutText(dst, str, cv::Point(aRect.x, aRect.y + 20), &font, color);
		}
		strcpy(str, "The total number of contours is: ");
		itoa(count, buf, 10);
		strcat(str, buf);
		color = CV_RGB((125 + rand()) + 15 & 255, (125 + rand()) & 255, (125 + rand()) & 255);//尽量偏亮的随机色
		cvPutText(dst, str, cvPoint(0, 120), &font, color);
		cvShowImage("Components", dst);
		cv::waitKey(31);
	}
	strcpy(str, "The treshhold is ");
	itoa(bar, buf, 10);
	strcat(str, buf);
	cvPutText(dst, str, cvPoint(0, 150), &font, color);
	cvShowImage("Components", dst);
	return;
}
int main(int argc, char** argv)
{
	cvInitFont(&font, CV_FONT_HERSHEY_SIMPLEX, 0.4, 0.4, 0);
	src = cvLoadImage("bin2.jpg", CV_LOAD_IMAGE_UNCHANGED);
	assert(src != NULL);
	cvNamedWindow("Source_Gray", 1);
	cvNamedWindow("Bin", 1);
	cvNamedWindow("Components", 1);
	gray = cvCreateImage(cvGetSize(src), 8, 1);
	//gray2 = cvCreateImage(cvGetSize(src), 8, 1);
	cvCvtColor(src, gray, CV_BGR2GRAY);
	bin = cvCreateImage(cvGetSize(gray), 8, 1);
	dst = cvCreateImage(cvGetSize(gray), 8, 3);
	cvXorS(gray, cvScalarAll(255), gray);
	cvShowImage("Source_Gray", gray);
	getBestTreshold();
 	color = CV_RGB(0,0,0);
	CvPoint c2, c3, c4;
	double r2 = 50;
	/*
	for (thta = 0; thta < PI * 2; thta = thta + PI / 32){
		gray2 = gray;///////////////////?
		c2.x = r1*cos(thta) + c1.x;
		c2.y = c1.y - r1*sin(thta);
		c3.x = r2*cos(thta + PI / 2) + c2.x;
		c3.y = c2.y - r2*sin(thta + PI / 2);
		c4.x = r2*cos(thta + PI * 3 / 2) + c2.x;
		c4.y = c2.y - r2*sin(thta + PI * 3 / 2);
		cvLine(gray2, c3, c4, color, 4, 8);
		cvThreshold(gray2, bin, bar, 255, CV_THRESH_BINARY);
		cvShowImage("Bin", bin);
		cvFindContours(bin, storage, &contour, sizeof(CvContour), CV_RETR_CCOMP, CV_CHAIN_APPROX_SIMPLE);//提取轮廓 
		cvZero(dst);//清空数组     
		color = CV_RGB(rand() & 255, rand() & 255, rand() & 255);//创建一个色彩值 
		int count = 0;
		flag = 0;
		for (; contour != 0 && flag == 0; contour = contour->h_next)
		{
			count++;
			double tmparea = fabs(cvContourArea(contour));//算面积
			CvRect aRect = cvBoundingRect(contour, 0);//算坐标与长宽
			//为消除过小连通域对画面产生的混乱感
			if (aRect.height < 105 || aRect.width < 105){
				cvSeqRemove(contour, 0); //删除某轮廓  
				continue;
			}
			if (aRect.height > 200 && aRect.width > 200 && tmparea > 20000 && aRect.height < 350){
				flag = 1;
				cout << "thta:" << thta << ",sub:" << maxarea - tmparea << endl;
				if (maxarea - tmparea > maxvalue){
					maxvalue = maxarea - tmparea;
					maxthta = thta;
				}
			}
			color = CV_RGB(0, 255, 255);
			cvDrawContours(dst, contour, color, color, -1, 1, 8);//绘制外部和内部的轮廓 
			color = CV_RGB(rand() & 255, rand() & 255, rand() & 255);//创建一个色彩值
			strcpy(str, "C");
			itoa(count, buf, 10);
			strcat(str, buf);
			strcat(str, " :Area= ");
			itoa(static_cast<int>(tmparea), buf, 10);
			strcat(str, buf);
			strcat(str, ",Width= ");
			itoa(aRect.width, buf, 10);
			strcat(str, buf);
			strcat(str, ",Height= ");
			itoa(aRect.height, buf, 10);
			strcat(str, buf);
			strcat(str, ".");
			cvPutText(dst, str, cv::Point(aRect.x, aRect.y + 20), &font, color);
		}
		strcpy(str, "The total number of contours is: ");
		itoa(count, buf, 10);
		strcat(str, buf);
		color = CV_RGB((125 + rand()) + 15 & 255, (125 + rand()) & 255, (125 + rand()) & 255);//尽量偏亮的随机色
		cvPutText(dst, str, cvPoint(0, 120), &font, color);
		cvShowImage("Components", dst);
	}
	*/
 	//cvShowImage("Bin", gray);
	cvWaitKey(0);
	//cvSaveImage("bin2.jpg", gray);
	cvDestroyWindow("Bin");
	cvReleaseImage(&bin);
	cvDestroyWindow("Source_Gray");
	cvReleaseImage(&gray);
	cvReleaseImage(&src);
	cvDestroyWindow("Components");
	cvReleaseImage(&dst);
	return 0;
}
