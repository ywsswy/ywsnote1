#include<iostream>
#include<opencv2/opencv.hpp>
#define YREGTEST
using namespace std;
using namespace cv;
void yRegister(Mat &imgrgb, Mat &fimgrgb, int &dx, int&dy, int type = -1, Point &plt = Point(0, 0), Point &prb = Point(0, 0));//总对齐
void yRegisterRough(Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul);//粗对齐
void yRegisterThorough(Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul);//细对齐
//细对齐
void yRegisterThorough(Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul){
	Mat imgA = fimggray(Rect(fimggray.cols / 3, fimggray.rows / 3, fimggray.cols / 3, fimggray.rows / 3));//模版图的中间1/3的感兴趣区域
	double fmean = mean(imgA)[0];//模版图中心1/3区域的平均“亮度”
	Mat imgB;//测试图的裁剪区域
	double xmean;//测试图裁剪区域的平均“亮度”
	int foundflag = 0;//0 还没有计算差值；1 已经计算了一次差值
	double xvalue;//减弱了测试图与模版图的比较区域平均“亮度”差异后的匹配差异
	double minvalue;//减弱了测试图与模版图的比较区域最匹配时平均“亮度”差异后的最匹配差异
	int minx, miny;//减弱了测试图与模版图的比较区域最匹配时平均“亮度”差异后的最匹配差异情况下的测试图区域左上角横纵坐标
	for (int i = fimggray.rows / 3 + (dy - 1)*mul; i < fimggray.rows / 3 + (dy + 1)*mul; i++){//考虑了粗对齐存在的误差
		for (int j = fimggray.cols / 3 + (dx - 1)*mul; j < fimggray.cols / 3 + (dx + 1)*mul; j++){
			if (j + fimggray.cols / 3>fimggray.cols || i + fimggray.rows / 3>fimggray.rows)//越界检查
				continue;
			imgB = imggray(Rect(j, i, fimggray.cols / 3, fimggray.rows / 3));
			xmean = mean(imgB)[0];
			//减去“亮度”差异后计算匹配差异
			if (xmean < fmean){
				Mat imgC(imgA.rows, imgA.cols, CV_8UC1, Scalar(static_cast<uchar>(fmean - xmean)));
				xvalue = mean(imgB + imgC - imgA)[0] + mean(imgA - imgB - imgC)[0];
			}
			else{
				Mat imgC(imgA.rows, imgA.cols, CV_8UC1, Scalar(static_cast<uchar>(xmean - fmean)));
				xvalue = mean(imgB - imgC - imgA)[0] + mean(imgA + imgC - imgB)[0];
			}
			//始终保存最匹配的位置信息
			if (foundflag == 0){
				foundflag = 1;
				minvalue = xvalue;
				minx = j;
				miny = i;
			}
			else if (minvalue > xvalue){
				minvalue = xvalue;
				minx = j;
				miny = i;
			}
		}
	}
	dx = minx - fimggray.cols / 3;
	dy = miny - fimggray.rows / 3;
#ifdef YREGTEST
	cout << "细对齐得出：测试图片可大约由模版图片向";
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
	imgB = imggray(Rect(minx, miny, fimggray.cols / 3, fimggray.rows / 3));
	int turnflag = 0;
	while ((waitKey(400)) != '\r'){
		if (turnflag == 0){
			imshow("对齐效果图", imgA);
		}
		else{
			imshow("对齐效果图", imgB);
		}
		turnflag = !turnflag;
	}
#endif
}
//粗对齐
void yRegisterRough(Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul){
	Mat imgA = fimggray(Rect(fimggray.cols / 3, fimggray.rows / 3, fimggray.cols / 3, fimggray.rows / 3));//模版图的中间1/3的感兴趣区域
	double fmean = mean(imgA)[0];//模版图中心1/3区域的平均“亮度”
	Mat imgB;//测试图的裁剪区域
	double xmean;//测试图裁剪区域的平均“亮度”
	int foundflag = 0;//0 还没有计算差值；1 已经计算了一次差值
	double xvalue;//减弱了测试图与模版图的比较区域平均“亮度”差异后的匹配差异
	double minvalue;//减弱了测试图与模版图的比较区域最匹配时平均“亮度”差异后的最匹配差异
	int minx, miny;//减弱了测试图与模版图的比较区域最匹配时平均“亮度”差异后的最匹配差异情况下的测试图区域左上角横纵坐标
	for (int i = 0; i < fimggray.rows * 2 / 3; i++){
		for (int j = 0; j < fimggray.cols * 2 / 3; j++){
			if (j + fimggray.cols / 3>fimggray.cols || i + fimggray.rows / 3>fimggray.rows)//越界检查
				continue;
			imgB = imggray(Rect(j, i, fimggray.cols / 3, fimggray.rows / 3));
			xmean = mean(imgB)[0];
			//减去“亮度”差异后计算匹配差异
			if (xmean < fmean){
				Mat imgC(imgA.rows, imgA.cols, CV_8UC1, Scalar(static_cast<uchar>(fmean - xmean)));
				xvalue = mean(imgB + imgC - imgA)[0] + mean(imgA - imgB - imgC)[0];
			}
			else{
				Mat imgC(imgA.rows, imgA.cols, CV_8UC1, Scalar(static_cast<uchar>(xmean - fmean)));
				xvalue = mean(imgB - imgC - imgA)[0] + mean(imgA + imgC - imgB)[0];
			}
			//始终保存最匹配的位置信息
			if (foundflag == 0){
				foundflag = 1;
				minvalue = xvalue;
				minx = j;
				miny = i;
			}
			else if (minvalue > xvalue){
				minvalue = xvalue;
				minx = j;
				miny = i;
			}
		}
	}
	dx = minx - fimggray.cols / 3;
	dy = miny - fimggray.rows / 3;
#ifdef YREGTEST
	cout << "粗对齐得出：测试图片可大约由模版图片向";
	if (dx < 0)
		cout << "左";
	else
		cout << "右";
	cout << "移动" << abs(dx*mul) << "个像素，向";
	if (dy < 0)
		cout << "上";
	else
		cout << "下";
	cout << "移动" << abs(dy*mul) << "个像素而来" << endl;
	imgB = imggray(Rect(minx, miny, fimggray.cols / 3, fimggray.rows / 3));
	int turnflag = 0;
	while ((waitKey(400)) != '\r'){
		if (turnflag == 0){
			imshow("对齐效果图", imgA);
		}
		else{
			imshow("对齐效果图", imgB);
		}
		turnflag = !turnflag;
	}
#endif
}
/*
总对齐
输入：
imgrgb：彩色测试图片
fimgrgb：彩色模版图片
type -1：不忽视模版中任何区域的对齐
	0：忽视模版中以plt为左上角，以prb为右下角的矩形区域的对齐
输出：
dx ：测试图片 相对于 模版图片 在水平方向的偏移像素
dy ：测试图片 相对于 模版图片 在垂直方向的偏移像素
可变参数：
mul 6：1920*1080时最佳效率
*/
void yRegister(Mat &imgrgb, Mat &fimgrgb, int &dx, int&dy, int type/* = -1*/, Point &plt/* = Point(0, 0)*/, Point &prb/* = Point(0, 0)*/){
	if (type == -1){
		int mul = 6;
		Mat imggray, fimggray;
		cvtColor(imgrgb, imggray, CV_BGR2GRAY);
		cvtColor(fimgrgb, fimggray, CV_BGR2GRAY);
		Mat rimggray, rfimggray;
		resize(imggray, rimggray, Size(imgrgb.cols / mul, imgrgb.rows / mul));
		resize(fimggray, rfimggray, Size(fimgrgb.cols / mul, fimgrgb.rows / mul));
		//粗对齐
		yRegisterRough(rimggray, rfimggray, dx, dy, mul);
		//细对齐
		yRegisterThorough(imggray, fimggray, dx, dy, mul);
	}
	else{
		//待补充有忽视干扰区域的情况
		exit(0);
	}
}
int main(){
#if 0
	string fimgpath = R"(pic\mod11.jpg)";//模版图片路径
	string imgpath = R"(pic\mm2.jpg)";//测试图片路径
#endif
#if 0
	string fimgpath = R"(pic\E1-3.jpg)";//模版图片路径
	string imgpath = R"(pic\2-3.jpg)";//测试图片路径
#endif
#if 0
	string fimgpath = R"(pic\f1.jpg)";//模版图片路径
	string imgpath = R"(pic\f2.jpg)";//测试图片路径
#endif
#if 0
	string fimgpath = R"(pic\T.JPG)";//模版图片路径
	string imgpath = R"(pic\test0.jpg)";//测试图片路径
#endif
#if 0
	string fimgpath = R"(pic\f3.jpg)";//模版图片路径
	string imgpath = R"(pic\f4.jpg)";//测试图片路径
#endif
#if 1
	string fimgpath = R"(pic\b1.jpg)";//模版图片路径
	string imgpath = R"(pic\b2.jpg)";//测试图片路径
#endif
	Mat img, fimg;
	int dx, dy;
	img = imread(imgpath);
	fimg = imread(fimgpath);
	yRegister(img, fimg, dx, dy);
	return 0;
}
