#include <opencv2\opencv.hpp>
#include <opencv2\imgproc\imgproc.hpp>
#include <opencv2\highgui\highgui.hpp>
#include <opencv\cv.h>
#include <opencv\cv.hpp>
#include <stdio.h>
using namespace std;
using namespace cv;
void cvRegister(int *dx, int*dy, Mat img4, Mat fimg4){
	int h = img4.rows;
	int w = img4.cols;
	Scalar avg;
	//cout << "w=" << w << ",h=" << h << endl;
	Mat imgA, imgB, imgC;
	//imgA:切割获取模版图片中间1/3的感兴趣区域
	//imgB:扫描切割获取测试图片的等大区域
	//imgC:imgA-imgB
	int mini, minj;
	double minvalue, value;
	//保存在(mini行,minj列)处对齐最匹配的程度(最小差值和)minvalue;
	imgA = img4(Rect(w / 3, h / 3, w / 3, h / 3));
	//imshow("模版中心图", imgA);
	//cout << "w=" << imgA.cols << ",h=" << imgA.rows << endl;
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = 0; i < h * 2 / 3; i++){
		for (int j = 0; j < w * 2 / 3; j++){
			value = 0;
			imgB = fimg4(Rect(j, i, w / 3, h / 3));
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
	//cout << "mini=" << mini << ",minj=" << minj << endl;
	imgB = fimg4(Rect(minj, mini, w / 3, h / 3));
	//imshow("对齐中心图", imgB);
	*dx = minj - w / 3;
	*dy = mini - h / 3;
	cout << "dx=" << *dx << ",dy=" << *dy << endl;
}
void cvRegister2(int *dx, int *dy, Mat img4, Mat fimg4,int mul){
	int h = img4.rows;
	int w = img4.cols;
	Scalar avg;
	//cout << "w=" << w << ",h=" << h << endl;
	Mat imgA, imgB, imgC;
	//imgA:切割获取模版图片中间1/3的感兴趣区域
	//imgB:扫描切割获取测试图片的等大区域
	//imgC:imgA-imgB
	int mini, minj;
	double minvalue, value;
	//保存在(mini行,minj列)处对齐最匹配的程度(最小差值和)minvalue;
	imgA = img4(Rect(w / 3, h / 3, w / 3, h / 3));
	imshow("模版中心图", imgA);
	//cout << "w=" << imgA.cols << ",h=" << imgA.rows << endl;
	minvalue = 255.0 * 2 * 3 * w*h / 3 / 3;
	for (int i = h/3+(*dy-1)*mul; i < h/3+(*dy+1)*mul; i++){
		for (int j = w/3+(*dx-1)*mul; j < w/3+(*dx+1)*mul; j++){
			value = 0;
			imgB = fimg4(Rect(j, i, w / 3, h / 3));
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
	//cout << "mini=" << mini << ",minj=" << minj << endl;
	imgB = fimg4(Rect(minj, mini, w / 3, h / 3));
	imshow("对齐中心图", imgB);
	*dx = minj - w / 3;
	*dy = mini - h / 3;
	cout << "dx=" << *dx << ",dy=" << *dy << endl;
	Mat imgD = imgB - imgA;
	imshow("相减图", imgD);
}
void cvRegist(Mat img4rgb,Mat fimg4rgb,int *dx,int*dy){
	Mat img4, fimg4;//原始模版与测试图片
	cvtColor(img4rgb, img4, CV_BGR2GRAY);
	cvtColor(fimg4rgb, fimg4, CV_BGR2GRAY);
	int mul = 6;
	Mat rimg4, rfimg4;
	resize(img4, rimg4, Size(img4.cols / mul, img4.rows / mul));
	resize(fimg4, rfimg4, Size(fimg4.cols / mul, fimg4.rows / mul));
	imshow("模版图", rimg4);
	imshow("测试图", rfimg4);
	cvRegister(dx,dy,rfimg4,rimg4);
	cvRegister2(dx, dy, fimg4, img4, mul);
	waitKey();
	return;
}
int main(){
	char ximg4[100] = "b2.jpg";//原始测试图片路径
	char xfimg4[100] = "b1.jpg";//原始模版图片路径
	Mat img4, fimg4;
	int dx, dy;
	img4 = imread(ximg4);
	fimg4 = imread(xfimg4);
	cvRegist(img4, fimg4,&dx,&dy);
}
