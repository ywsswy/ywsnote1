#include <opencv2/opencv.hpp>
#include <iostream>
#include <string>
#include <windows.h>
#define YREGTEST
using namespace std;
using namespace cv;
void yRegisterPre(Mat &imgrgb, Mat &fimgrgb, int &dx, int&dy, int type = -1, Point &plt = Point(0, 0), Point &prb = Point(0, 0));//对齐预处理
void yRegister(int stage, Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul, Point &plt, Point &prb);//对齐（stage：1粗对齐，2细对齐）
int main() {
	int dx, dy;
	Mat img1 = imread("it.jpg");
	Mat img2 = imread("im.jpg");
	yRegisterPre(img1, img2, dx, dy);
	return 0;
}
//对齐（stage：1粗对齐，2细对齐）
void yRegister(int stage,Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul, Point &plt, Point &prb) {
#ifdef YREGTEST
	imshow((stage == 1 ? ("粗") : ("细")) + string("对齐前模板图"), fimggray);
	imshow((stage == 1 ? ("粗") : ("细")) + string("对齐前模板图"), imggray);
#endif
	Mat imgA = fimggray(Rect(plt.x, plt.y, prb.x - plt.x, prb.y - plt.y));//模板图的欲对齐区域
	Mat imgB;//测试图的裁剪区域
	int foundflag = 0;//0 还没有计算差值；1 已经计算了一次差值
	double xvalue;//当前直方图的欧拉距离
	double minvalue;//最匹配的直方图的欧拉距离
	int minx = 0, miny = 0;//最匹配的直方图的欧拉距离情况下 的测试图区域左上角横纵坐标
	int i = 0;//行y
	int j = 0;//列x
	//循环变量i初始化
	i = (stage == 1 ? 0 : plt.y + (dy - 1)*mul);
	while (true) {
		//i退出条件
		if ((stage == 2 && i >= plt.y + (dy + 1)*mul)
			||
			(stage == 1 && i >= imggray.rows - (prb.y - plt.y))){
			break;
		}
		//循环变量j初始化
		j = (stage == 1 ? 0 : plt.x + (dx - 1)*mul);
		while (true) {
			//j退出条件
			if ((stage == 2 && j >= plt.x + (dx + 1)*mul)
				||
				(stage == 1 && j >= imggray.cols - (prb.x - plt.x))){
				break;
			}
			if (j + prb.x - plt.x > imggray.cols
				||
				j + prb.x - plt.x<0
				||
				i + prb.y - plt.y>imggray.rows
				||
				i + prb.y - plt.y < 0
				||
				i < 0
				||
				j < 0) {//越界检查
				continue;
			}
			imgB = imggray(Rect(j, i, prb.x - plt.x, prb.y - plt.y));
			Mat histB,histA;
			int histBinNum = 100;
			float range[] = { 0, 255 };
			const float* histRange = { range };
			calcHist(&imgA, 1, 0, Mat(), histA, 1, &histBinNum, &histRange);
			calcHist(&imgB, 1, 0, Mat(), histB, 1, &histBinNum, &histRange);
			xvalue = norm(histA, histB, CV_L2);
			//始终保存最匹配的位置信息
			if (foundflag == 0) {
				foundflag = 1;
				minvalue = xvalue;
				minx = j;
				miny = i;
			}
			else if (minvalue > xvalue) {
				minvalue = xvalue;
				minx = j;
				miny = i;
			}
			//循环后的处理
			j++;
		}
		i++;	
	}
	dx = minx - plt.x;
	dy = miny - plt.y;
#ifdef YREGTEST
	cout << (stage == 1 ? ("粗"):("细")) << "对齐得出：测试图片可大约由模板图片向";
	if (dx < 0)
		cout << "左";
	else
		cout << "右";
	cout << "移动" << abs(dx) << "个像素，向";
	if (dy < 0)
		cout << "上";
	else
		cout << "下";
	cout << "移动" << abs(dy) << "个像素而来" << endl;
	imgB = imggray(Rect(minx, miny, prb.x - plt.x, prb.y - plt.y));
	int turnflag = 0;
	while ((waitKey(400)) != '\r') {
		if (turnflag == 0) {
			imshow("对齐效果图", imgA);
		}
		else {
			imshow("对齐效果图", imgB);
		}
		turnflag = !turnflag;
	}
#endif
}
/*
imgrgb 原测试图
fimgrgb 原模板图
*/
void yRegisterPre(Mat &imgrgb, Mat &fimgrgb, int &dx, int&dy, int type/* = -1*/, Point &plt/* = Point(0, 0)*/, Point &prb/* = Point(0, 0)*/) {
	int mul = 6;
	Mat imggray, fimggray;
	cvtColor(imgrgb, imggray, CV_BGR2GRAY);
	cvtColor(fimgrgb, fimggray, CV_BGR2GRAY);
	Mat rimggray, rfimggray;
	resize(imggray, rimggray, Size(imgrgb.cols / mul, imgrgb.rows / mul));
	resize(fimggray, rfimggray, Size(fimgrgb.cols / mul, fimgrgb.rows / mul));
	int stage = 1;//1粗对齐，2细对齐
	if (type == -1) {
		//MyLog(DEBUGLEVEL, "yRegister：Y3-3 中间三分之一对齐");
		//粗对齐//模板图的中间1/3的感兴趣区域
		yRegister(stage,rimggray, rfimggray, dx, dy, mul, Point(rfimggray.cols / 3, rfimggray.rows / 3), Point(rfimggray.cols * 2 / 3, rfimggray.rows * 2 / 3));
		//MyLog(DEBUGLEVEL, "yRegister：Y3-3 粗对齐结束");
		stage++;
		//细对齐//模板图的中间1/3的感兴趣区域
		yRegister(stage,imggray, fimggray, dx, dy, mul, Point(fimggray.cols / 3, fimggray.rows / 3), Point(fimggray.cols * 2 / 3, fimggray.rows * 2 / 3));
		//MyLog(DEBUGLEVEL, "yRegister：Y3-3 细对齐结束");
	}
	else {
		//MyLog(DEBUGLEVEL, "yRegister：Y3-3 自定义区域对齐");
		//粗对齐//自定义区域
		yRegister(stage,rimggray, rfimggray, dx, dy, mul, Point(plt.x / mul, plt.y / mul), Point(prb.x / mul, prb.y / mul));
		stage++;
		//细对齐//自定义区域
		yRegister(stage,imggray, fimggray, dx, dy, mul, Point(plt.x, plt.y), Point(prb.x, prb.y));
	}
	//MyLog(DEBUGLEVEL, "yRegister:Y3-4 对齐结束");
}
