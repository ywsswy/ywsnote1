//A1 109,106
//46,55
//3969 2601
//r = 81
//ir = 70
//75,2,78%,4,0.1*9
//3249 
//////////////////////////
#include <opencv2/opencv.hpp>
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <iostream>
#include <vector>
#include<fstream> 
#include "GraphUtils.h"
const double PI = 3.14159265;
using namespace cv;
using namespace std;
static int nThreshold1 = 42;
static int nThreshold2 = 2;
static int nThreshold3 = 0;
static int nThreshold4 = 4;
static int nThreshold5 = 9;
Mat img;
Mat Kern, Out;
Mat Yan;
CvSize s1;
CvScalar color;
float comp[360] = { 0 };
void on_trackbar(int pos){
	double thta = 0.01 * PI * nThreshold3;
	double gama = 0.1 * nThreshold5;
	s1 = Size(nThreshold1, nThreshold1);
	Kern = getGaborKernel(s1, 1.*nThreshold2, thta, 1.*nThreshold4, gama);
	//imshow("kern", Kern);
	filter2D(img, Out, -1, Kern);
	//imshow("out", Out);
	Out = Out.mul(Yan);
	//imshow("rout", Out);
}
Mat yanmo(Mat gray){
	//灰度化图片
	Mat Yan;
	threshold(gray, Yan, 0, 255, THRESH_BINARY | THRESH_OTSU);//获得与原图像相同大小的二值图像
	int Cx = 109;                // 竖移 //计算圆心位置
	int Cy = 108;        //横移
	int Circle = 79;                   //设定掩膜圆的半径
	int inncir = 70;
	uchar *p;
	//通过访问像素点，此像素点如果在指定圆内像素值设为1，其他地方像素值设为0
	for (int i = 0; i < Yan.rows; i++){
		p = Yan.ptr<uchar>(i);
		for (int j = 0; j < Yan.cols; j++){
			if (i<Cy-10){
				if (((i - Cx)*(i - Cx) + (j - Cy)*(j - Cy))>(Circle*Circle) || ((i - Cx)*(i - Cx) + (j - Cy)*(j - Cy)) < (inncir*inncir)){
					p[j] = 0;
				}
				else{
					p[j] = 1;
				}
			}
			else{
				p[j] = 0;
			}
		}
	}
	//imshow("掩膜", Yan * 255);
	return Yan;
}
int main(){
	//cvNamedWindow("Test", 1);
	//kernelsize 接受区域的大小σ 方向（0为垂直) 波长（像素/宽度) 纵横比（伸展长度）
	img = imread("A1.jpg", CV_LOAD_IMAGE_GRAYSCALE);
	//imshow("Original Image", img);
	Yan = yanmo(img);
// 	cvCreateTrackbar("size", "Test", &nThreshold1, 100, on_trackbar);
// 	cvCreateTrackbar("σ", "Test", &nThreshold2, 40, on_trackbar);
// 	cvCreateTrackbar("θ（π%）", "Test", &nThreshold3, 100, on_trackbar);
// 	cvCreateTrackbar("λ", "Test", &nThreshold4, 10, on_trackbar);
// 	cvCreateTrackbar("0.1*γ", "Test", &nThreshold5, 40, on_trackbar);
	//on_trackbar(NULL);
	double thta = 0.01 * PI * nThreshold3;
	double gama = 0.1 * nThreshold5;
	s1 = Size(nThreshold1, nThreshold1);
	uchar *pt;      //创建指针
	for (nThreshold3 = 0; nThreshold3 < 180; nThreshold3++){
		comp[nThreshold3] = 0;
		Kern = getGaborKernel(s1, 1.*nThreshold2, PI /180.0*nThreshold3, 1.*nThreshold4, gama);
		//imshow("kern", Kern);
		filter2D(img, Out, -1, Kern);
		//imshow("out", Out);
		Out = Out.mul(Yan);
		//imshow("rout", Out);
		for (int row = 0; row < Out.rows; row++){
			pt = Out.ptr<uchar>(row);
			for (int col = 0; col < Out.cols; col++){
				int a = pt[col];
				//cout << "此像素点数值为" << a << endl;
				if (a){
					//cout << "统计中的数值" << counts[th] << endl;
					comp[nThreshold3] += a;
				}
			}
		}
		//cout << "角度为" << nThreshold3 << "的响应值为" << comp[nThreshold3] << endl;
	}
	IplImage *bgImg = cvLoadImage("grid.jpg");
	drawFloatGraph(comp, 180, bgImg, 1000.0F,6000.0F, bgImg->width, bgImg->height);
	//showFloatGraph("响应折线图", comp, 360, 0);
	//showImage(bgImg);
	int max = 0;
	double value = 0;
	for (int i = 0; i < 180; i++){
		if (comp[i]>value){
			max = i;
			value = comp[i];
		}
	}
	cout << "检测角度：" << max << "，转换值：0.1"<< endl;
	color = CV_RGB(rand() & 255, rand() & 255, rand() & 255);//创建一个色彩值
	//CvArr* s = (CvArr*)&img;
	//cvLine(s, cvPoint(37, 46), cvPoint(82, 88), color,3);
	Mat imgrgb = imread("A1.jpg");
	imshow("origin", imgrgb);
	line(imgrgb, cvPoint(37, 46), cvPoint(82, 88), color, 3);
	imshow("result", imgrgb);
	cvWaitKey(0);
	//cvDestroyWindow("Test");
}
