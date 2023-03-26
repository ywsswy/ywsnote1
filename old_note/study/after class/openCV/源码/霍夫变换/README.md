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
using namespace cv;
void rotate_test()
{
}
static int n1 = 0;
static int n2 = 1;
static int n3 = 0;
static int n4 = 0;
Mat		image(240, 320, CV_8U, Scalar(0));
typedef struct CvLinePolar
{
	float rho;
	float angle;
} CvLinePolar;
static void icvHoughLinesStandard(const CvMat* img, float rho, float theta,int threshold, CvSeq *lines, int linesMax)
{
	cv::AutoBuffer<int> _accum, _sort_buf;
	cv::AutoBuffer<float> _tabSin, _tabCos;
	const uchar* image;
	int step, width, height;
	int numangle, numrho;
	int total = 0;
	float ang;
	int r, n;
	int i, j;
	float irho = 1 / rho;
	double scale;
	CV_Assert(CV_IS_MAT(img) && CV_MAT_TYPE(img->type) == CV_8UC1);
	image = img->data.ptr;
	step = img->step;
	width = img->cols;
	height = img->rows;
	numangle = cvRound(CV_PI / theta);    // 霍夫空间，角度方向的大小    
	numrho = cvRound(((width + height) * 2 + 1) / rho);  // r的空间范围    
	_accum.allocate((numangle + 2) * (numrho + 2));
	_sort_buf.allocate(numangle * numrho);
	_tabSin.allocate(numangle);
	_tabCos.allocate(numangle);
	int *accum = _accum, *sort_buf = _sort_buf;
	float *tabSin = _tabSin, *tabCos = _tabCos;
	memset(accum, 0, sizeof(accum[0]) * (numangle + 2) * (numrho + 2));
	for (ang = 0, n = 0; n < numangle; ang += theta, n++) // 计算正弦曲线的准备工作，查表    
	{
		tabSin[n] = (float)(sin(ang) * irho);
		tabCos[n] = (float)(cos(ang) * irho);
	}
	// stage 1. fill accumulator    
	for (i = 0; i < height; i++)
	for (j = 0; j < width; j++)
	{
		if (image[i * step + j] != 0)      // 将每个非零点，转换为霍夫空间的离散正弦曲线，并统计。    
		for (n = 0; n < numangle; n++)
		{
			r = cvRound(j * tabCos[n] + i * tabSin[n]);
			r += (numrho - 1) / 2;
			accum[(n + 1) * (numrho + 2) + r + 1]++;
		}
	}
	// stage 2. find local maximums  // 霍夫空间，局部最大点，采用四邻域判断，比较。（也可以使8邻域或者更大的方式）, 如果不判断局部最大值，同时选用次大值与最大值，就可能会是两个相邻的直线，但实际上是一条直线。选用最大值，也是去除离散的近似计算带来的误差，或合并近似曲线。    
	for (r = 0; r < numrho; r++)
	for (n = 0; n < numangle; n++)
	{
		int base = (n + 1) * (numrho + 2) + r + 1;
		if (accum[base] > threshold &&
			accum[base] > accum[base - 1] && accum[base] >= accum[base + 1] &&
			accum[base] > accum[base - numrho - 2] && accum[base] >= accum[base + numrho + 2])
			sort_buf[total++] = base;
	}
	// stage 3. sort the detected lines by accumulator value    // 由点的个数排序，依次找出哪些最有可能是直线    
	icvHoughSortDescent32s(sort_buf, total, accum);
	// stage 4. store the first min(total,linesMax) lines to the output buffer    
	linesMax = MIN(linesMax, total);
	scale = 1. / (numrho + 2);
	for (i = 0; i < linesMax; i++)  // 依据霍夫空间分辨率，计算直线的实际r，theta参数    
	{
		CvLinePolar line;
		int idx = sort_buf[i];
		int n = cvFloor(idx*scale) - 1;
		int r = idx - (n + 1)*(numrho + 2) - 1;
		line.rho = (r - (numrho - 1)*0.5f) * rho;
		line.angle = n * theta;
		cvSeqPush(lines, &line);
	}
}
void on_trackbar(int pos){
	Point   center(n3,n4);
	Mat R = getRotationMatrix2D(center, float(n1), (float)n2);
	Mat imgR;
	warpAffine(image, imgR, R, Size(320, 240));
	imshow("Rotate image", imgR);
}
int main(){
	cvNamedWindow("age", 1);
	rectangle(image, Rect(80, 60, 100, 50), Scalar(255), CV_FILLED);
	imshow("Image", image);
	cvCreateTrackbar("angle", "age", &n1, 360, on_trackbar);
	cvCreateTrackbar("scale", "age", &n2, 4, on_trackbar);
	cvCreateTrackbar("x", "age", &n3, 320, on_trackbar);
	cvCreateTrackbar("y", "age", &n4, 240, on_trackbar);
	on_trackbar(NULL);
	cvWaitKey(0);
	cvDestroyWindow("age");
	return 0;
}
