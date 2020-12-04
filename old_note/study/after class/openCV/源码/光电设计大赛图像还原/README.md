#include <opencv2/opencv.hpp>
#include <iostream>
#include <string>
#include <algorithm>
#include <list>
#include <time.h>
using namespace std;
using namespace cv;
/*
抠图
曝光时间
剪背景
*/
/*
0.969 qingxidu
(double(814)/359) xiangsuzhijing
4 jiange
85 resize daxiao
47 422
*/
#define YKER 12
#define YRUO (0.98)
#define YMUL (double(814)/422)
//354
#define YEDGE (int)(814/YMUL)
#define YDDD 5
#define YSSS 80
#define YEDG11 (int)(166/YMUL)
#define YEDG21 (int)(206/YMUL)
#define YEDG31 (int)(289/YMUL)
#define YEDG41 (int)(305/YMUL)
#define YEDG51 (int)(350/YMUL)
#define YEDG61 (int)(368/YMUL)
#define YEDG71 (int)(388/YMUL)
#define YEDG8 (int)(410/YMUL)
#define YEDG12 (int)(649/YMUL)
#define YEDG22 (int)(612/YMUL)
#define YEDG32 (int)(530/YMUL)
#define YEDG42 (int)(510/YMUL)
#define YEDG52 (int)(469/YMUL)
#define YEDG62 (int)(450/YMUL)
#define YEDG72 (int)(430/YMUL)
#define YTEAM_NAME "A27"
#define YBUTTON_OPEN_NAME "TEAM_SHOW"
#define YBUTTON_CLOSE_NAME "CLOSENAME"
#define YBUTTON_SAVE1_NAME "TEST"
#define YBUTTON_SAVE2_NAME "SAVE"
#define YBUTTON_SAVE12_NAME "BEGIN"
#define YBUTTON_SAVE12_END_NAME "END  "
#define YBUTTON_SUB_NAME "TEST"
#define YIS_SHOW 1
#define YNOT_SHOW 0
#define YWIN_NAME "A28"
#define YDWIN_NAME "读取视频"
#define YCAMERA_NUM 1
#define YWIN_H 700
#define YWIN_W 480*2
#define YDEFPIC_W 640
#define YDEFPIC_H 480
#define YIMG_GRAY_X 0
#define YIMG_GRAY_Y 20
#define YIMG_GRAY_H 480
#define YIMG_GRAY_W 480
#define YFONT_SIZE 2
#define YFONT_H 23
#define YFONT_W 21
#define YFONT_B 4
#define YDL 10
#define YBUTTON_SAVE12_X (YWIN_W*2/3)
#define YBUTTON_SAVE12_Y 570
#define YBUTTON_TEAM_X (YWIN_W*2/3)
#define YBUTTON_TEAM_Y 610
#define YBUTTON_SAVE1_X (YWIN_W*1/3)
#define YBUTTON_SAVE1_Y 570
#define YBUTTON_SAVE2_X (YWIN_W*1/3)
#define YBUTTON_SAVE2_Y 610
#define YBUTTON_SUB_X 0
#define YBUTTON_SUB_Y 570
#define YLABEL1_X (YIMG_GRAY_X + YIMG_GRAY_W/2)
#define YLABEL1_Y (YIMG_GRAY_Y + YIMG_GRAY_H)
#define YDIA 363
#define YBIGLEN 65
VideoCapture capture(YCAMERA_NUM);
Mat winimg(YWIN_H, YWIN_W, CV_8UC3, Scalar(22, 222, 22));
Mat imggray;
Mat dir;
vector<Mat> channels;
int flag_show = YNOT_SHOW;//1：显示队伍编号
int flag_sub = YNOT_SHOW;//1：显示确定的中心位置
int flagstart = 0;//实时采集并显示 1：开始计算，停止采集
int flagh1 = 0;//1：外环及中心已确定
int flagh2 = 0;//1：第三环已确定
int flagh3 = 0;//1：第四环已确定
list<int> li;//消除影响
int ang[4] = { 0 };//最外环的角度（-135~180）到最内环
int rang[4] = { 0 };
int dis[8][2] = { 0 };
int px = YIMG_GRAY_W / 2;
int py = YIMG_GRAY_H / 2;
//绘制按键界面
void yDrawButLab() {
	{
		Mat white1(YFONT_H + YFONT_B, YFONT_W*string(YBUTTON_OPEN_NAME).length(), CV_8UC3, Scalar(222, 222, 222));
		Mat imageROI = winimg(Rect(YBUTTON_TEAM_X, YBUTTON_TEAM_Y, YFONT_W*string(YBUTTON_OPEN_NAME).length(), YFONT_H + YFONT_B));
		white1.copyTo(imageROI);
	}
	{
		Mat white1(YFONT_H + YFONT_B, YFONT_W*string(YBUTTON_SAVE12_END_NAME).length(), CV_8UC3, Scalar(222, 222, 222));
		Mat imageROI = winimg(Rect(YBUTTON_SAVE12_X, YBUTTON_SAVE12_Y, YFONT_W*string(YBUTTON_SAVE12_END_NAME).length(), YFONT_H + YFONT_B));
		white1.copyTo(imageROI);
	}
	if (flagstart == 0) {
		putText(winimg, YBUTTON_SAVE12_NAME, Point(YBUTTON_SAVE12_X, YBUTTON_SAVE12_Y + YFONT_H), FONT_HERSHEY_SIMPLEX, 1, Scalar(255, 0, 0), YFONT_SIZE);
	}
	else {
		putText(winimg, YBUTTON_SAVE12_END_NAME, Point(YBUTTON_SAVE12_X, YBUTTON_SAVE12_Y + YFONT_H), FONT_HERSHEY_SIMPLEX, 1, Scalar(255, 0, 0), YFONT_SIZE);
	}
	putText(winimg, YBUTTON_SAVE1_NAME, Point(YBUTTON_SAVE1_X, YBUTTON_SAVE1_Y + YFONT_H), FONT_HERSHEY_SIMPLEX, 1, Scalar(255, 0, 0), YFONT_SIZE);
	putText(winimg, YBUTTON_SAVE2_NAME, Point(YBUTTON_SAVE2_X, YBUTTON_SAVE2_Y + YFONT_H), FONT_HERSHEY_SIMPLEX, 1, Scalar(255, 0, 0), YFONT_SIZE);
	putText(winimg, YBUTTON_SUB_NAME, Point(YBUTTON_SUB_X, YBUTTON_SUB_Y + YFONT_H), FONT_HERSHEY_SIMPLEX, 1, Scalar(255, 0, 0), YFONT_SIZE);
	if (flag_show == YNOT_SHOW) {
		putText(winimg, YBUTTON_OPEN_NAME, Point(YBUTTON_TEAM_X, YBUTTON_TEAM_Y + YFONT_H), FONT_HERSHEY_SIMPLEX, 1, Scalar(255, 0, 0), YFONT_SIZE);
		Mat white1(YFONT_H + YFONT_B, YFONT_W*string(YTEAM_NAME).length(), CV_8UC3, Scalar(222, 222, 222));
		{
			{
				Mat imageROI = winimg(Rect(YLABEL1_X, YLABEL1_Y, YFONT_W*string(YTEAM_NAME).length(), YFONT_H + YFONT_B));
				white1.copyTo(imageROI);
			}
		}
	}
	else {
		putText(winimg, YBUTTON_CLOSE_NAME, Point(YBUTTON_TEAM_X, YBUTTON_TEAM_Y + YFONT_H), FONT_HERSHEY_SIMPLEX, 1, Scalar(255, 0, 0), YFONT_SIZE);
		putText(winimg, YTEAM_NAME, Point(YLABEL1_X, YLABEL1_Y + YFONT_H), FONT_HERSHEY_SIMPLEX, 1, Scalar(0, 0, 255), YFONT_SIZE);
	}
	return;
}
//判断是否按键在范围内
bool yJudgeIn(int ix, int iy, int x, int y, int w, int h) {
	if (ix >= x
		&&
		ix <= x + w
		&&
		iy >= y
		&&
		iy <= y + h) {
		return true;
	}
	else {
		return false;
	}
}
//给img加type(1~4 外到内)的angle黑环
void yRot(int type, Mat &img, int angle) {
	Mat white(YEDGE, YEDGE, CV_8UC1, Scalar(0));
	double x = 0;
	double y = 0;
	double w = 0;
	double h = 0;
	if (type == 1) {
		x = YEDGE / 2 - (YEDG11 - 0 + YEDGE - YEDG12) / 4;
		y = 0;
		w = (YEDG11 - 0 + YEDGE - YEDG12) / 2;
		h = (YEDG11 - 0 + YEDGE - YEDG12) / 2 + (YEDG12 - YEDG11) / 2 - sqrt(pow(((YEDG12 - YEDG11) / 2), 2) - pow((YEDG11 - 0 + YEDGE - YEDG12) / 4, 2)) + 3;
	}
	else if (type == 2) {
		x = YEDGE / 2 - (YEDG31 - YEDG21 + YEDG22 - YEDG32) / 4;
		y = YEDGE / 2 - (YEDG22 - YEDG21) / 2;
		w = (YEDG31 - YEDG21 + YEDG22 - YEDG32) / 2;
		h = (YEDG31 - YEDG21 + YEDG22 - YEDG32) / 2 + (YEDG32 - YEDG31) / 2 - sqrt(pow(((YEDG32 - YEDG31) / 2), 2) - pow((YEDG31 - YEDG21 + YEDG22 - YEDG32) / 4, 2)) + 3;
	}
	else if (type == 3) {
		x = YEDGE / 2 - (YEDG51 - YEDG41 + YEDG42 - YEDG52) / 4;
		y = YEDGE / 2 - (YEDG42 - YEDG41) / 2;
		w = (YEDG51 - YEDG41 + YEDG42 - YEDG52) / 2;
		h = (YEDG51 - YEDG41 + YEDG42 - YEDG52) / 2 + (YEDG52 - YEDG51) / 2 - sqrt(pow(((YEDG52 - YEDG51) / 2), 2) - pow((YEDG51 - YEDG41 + YEDG42 - YEDG52) / 4, 2)) + 3;
	}
	else if (type == 4) {
		x = YEDGE / 2 - (YEDG71 - YEDG61 + YEDG62 - YEDG72) / 4;
		y = YEDGE / 2 - (YEDG62 - YEDG61) / 2;
		w = (YEDG71 - YEDG61 + YEDG62 - YEDG72) / 2;
		h = (YEDG71 - YEDG61 + YEDG62 - YEDG72) / 2 + (YEDG72 - YEDG71) / 2 - sqrt(pow(((YEDG72 - YEDG71) / 2), 2) - pow((YEDG71 - YEDG61 + YEDG62 - YEDG72) / 4, 2)) + 3;
	}
	rectangle(white, Rect((int)x, (int)y, (int)w, (int)h), Scalar(255));
	floodFill(white, Point2i((int)x + 4, (int)y + 4), 255);
	Point center = Point(YEDGE / 2, YEDGE / 2);
	Mat rotMat = getRotationMatrix2D(center, -angle, 1.0);
	warpAffine(white, white, rotMat, white.size());
	img = white | img;
	return;
}
//求浮点数四舍五入
int ySiS(double a) {
	if (a > 0) {
		return (int)(a + 0.5);
	}
	else {
		return (int)(a - 0.5);
	}
}
//图像增强，最后显示部分
void yFun(Mat &imgbin) {
	Mat img(YEDGE, YEDGE, CV_8UC1, Scalar(255));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), YEDGE / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG12 - YEDG11) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG22 - YEDG21) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG32 - YEDG31) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG42 - YEDG41) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG52 - YEDG51) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG62 - YEDG61) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG72 - YEDG71) / 2, Scalar(0));
	//上4个
	floodFill(img, Point2i(YEDG11 - 5, YEDGE / 2), 0);
	floodFill(img, Point2i(YEDG31 - 5, YEDGE / 2), 0);
	floodFill(img, Point2i(YEDG51 - 5, YEDGE / 2), 0);
	floodFill(img, Point2i(YEDG71 - 5, YEDGE / 2), 0);
	for (int i = 0; i < 4; i++) {
		yRot(i + 1, img, rang[i]);
	}
	for (int j = 0; j < img.rows; j++)
	{
		for (int i = 0; i < img.cols; i++)
		{
			if (img.at<unsigned char>(j, i) == 0) {
				imgbin.at<unsigned char>(j, i) *= YRUO;
			}
		}
	}
	return;
}
//保存处理图
void yPr() {
	Mat imgROI = imggray(Rect(px - (YEDGE / 2), py - (YEDGE / 2), YEDGE, YEDGE));
	Mat imgbuf;
	imgROI.copyTo(imgbuf);
	//保存最终处理图
	yFun(imgbuf);
	blur(imgbuf, imgbuf, Size(YKER, YKER));
	Mat dir(imgbuf.rows, imgbuf.cols, CV_8UC1);
	//enhance(imgbuf, dir);
	imgbuf.copyTo(dir);
	Mat imgbuf2;
	resize(dir, imgbuf2, Size(500, 500));
	putText(imgbuf2, YTEAM_NAME, Point(420, 480), FONT_HERSHEY_SIMPLEX, 1, Scalar(125), 2);
	string ss = YTEAM_NAME;
	ss = ss + "-xcm.bmp";
	imwrite(ss, imgbuf2);
	Mat iii;
	imgROI.copyTo(iii);
	resize(iii, iii, Size(500, 500));
	putText(iii, YTEAM_NAME, Point(420, 480), FONT_HERSHEY_SIMPLEX, 1, Scalar(125), 2);
	imwrite("orgin.bmp", iii);
	cout << "已保存" << endl;
}
//保存yc（可加margin）
void yYu() {
	Mat img(YEDGE, YEDGE, CV_8UC1, Scalar(255));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), YEDGE / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG12 - YEDG11) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG22 - YEDG21) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG32 - YEDG31) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG42 - YEDG41) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG52 - YEDG51) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG62 - YEDG61) / 2, Scalar(0));
	circle(img, Point2i(YEDGE / 2, YEDGE / 2), (YEDG72 - YEDG71) / 2, Scalar(0));
	//上4个
	floodFill(img, Point2i(YEDG11 - 5, YEDGE / 2), 0);
	floodFill(img, Point2i(YEDG31 - 5, YEDGE / 2), 0);
	floodFill(img, Point2i(YEDG51 - 5, YEDGE / 2), 0);
	floodFill(img, Point2i(YEDG71 - 5, YEDGE / 2), 0);
	for (int i = 0; i < 4; i++) {
		yRot(i + 1, img, rang[i]);
	}
	Mat imgbuf;
	resize(img, imgbuf, Size(500, 500));
	putText(imgbuf, YTEAM_NAME, Point(420, 480), FONT_HERSHEY_SIMPLEX, 1, Scalar(125), 2);
	string ss = YTEAM_NAME;
	ss = ss + "-xcm.bmp";
	imwrite(ss, imgbuf);
	cout << "已保存" << endl;
	return;
}
//图像增强2，最后提交部分
void  enhance(Mat src, Mat dir)
{
	int srcpixel[256] = { 0 };
	int dirpixel[256] = { 0 };
	int p;//temp
	double srcprob[256];
	double dirprob[256] = { 0 };
	double zhong = src.rows*src.cols;
	for (int i = 0; i < src.rows; i++)
	{
		for (int j = 0; j < src.cols; j++)
		{
			p = src.at<uchar>(i, j);
			srcpixel[p]++;
		}
	}
	for (int i = 0; i < 255; i++)
	{
		srcprob[i] = srcpixel[i] / zhong;
	}
	double o = 0;
	dirprob[0] = srcprob[0];
	for (int i = 1; i < 256; i++)
	{
		dirprob[i] = dirprob[i - 1] + srcprob[i];
	}
	for (int i = 0; i < src.rows; i++)
	{
		for (int j = 0; j < src.cols; j++)
		{
			p = src.at<uchar>(i, j);
			dir.at<uchar>(i, j) = 255 * dirprob[p];
		}
	}
}
//鼠标的回调函数
void on_MouseHandle(int event, int x, int y, int flags, void* param)
{
	Mat& image = *(cv::Mat*) param;
	switch (event)
	{
	case EVENT_MOUSEMOVE:
		if (flagh2 == 1 && flagh3 == 0) {
			//cout << "满足条件1 " << endl;
			double len = sqrt((pow(x - dis[rang[2] / 45 + 3][0], 2)) + (pow(y - dis[rang[2] / 45 + 3][1], 2)));
			if (len > YBIGLEN) {
				flagh3 = 1;
				int dx = x - dis[rang[2] / 45 + 3][0];
				int dy = y - dis[rang[2] / 45 + 3][1];
				dy = -dy;
				int maxd = max(abs(dx), abs(dy));
				dx = ySiS((double)dx / maxd);
				dy = ySiS((double)dy / maxd);
				if (dx == 1) {
					if (dy == 1) {
						rang[3] = 45;
					}
					else if (dy == 0) {
						rang[3] = 90;
					}
					else {
						rang[3] = 135;
					}
				}
				else if (dx == 0) {
					if (dy == 1) {
						rang[3] = 0;
					}
					else {
						rang[3] = 180;
					}
				}
				else {
					if (dy == 1) {
						rang[3] = -45;
					}
					else if (dy == 0) {
						rang[3] = -90;
					}
					else {
						rang[3] = -135;
					}
				}
				//cout << "计算完毕" << endl;
			}
		}
		break;
	case EVENT_LBUTTONDOWN:
		if (yJudgeIn(x, y, YBUTTON_TEAM_X, YBUTTON_TEAM_Y, YFONT_W*string(YBUTTON_OPEN_NAME).length(), YFONT_H)) {
			flag_show = (flag_show + 1) % 2;
			for (int i = 0; i < 8; i++) {
				dis[i][0] = YBUTTON_TEAM_X + YFONT_W*(1.5 + i);
				dis[i][1] = YBUTTON_TEAM_Y + YFONT_H*(0.5);
				if (yJudgeIn(x, y, YBUTTON_TEAM_X + YFONT_W*(1 + i), YBUTTON_TEAM_Y, YFONT_W, YFONT_H)) {
					if (flag_sub == 0) {
						rang[2] = (i - 3) * 45;
					}
					else {
						rang[1] = (i - 3) * 45;
					}
					flagh2 = 1;
					flagh3 = 0;
					cout << "已切换队伍名称" << endl;
				}
			}
		}
		else if (yJudgeIn(x, y, YBUTTON_SAVE12_X, YBUTTON_SAVE12_Y, YFONT_W*string(YBUTTON_SAVE12_NAME).length(), YFONT_H)) {
			//start
			flagstart = (flagstart + 1) % 2;
		}
		else if (yJudgeIn(x, y, YBUTTON_SAVE1_X, YBUTTON_SAVE1_Y, YFONT_W*string(YBUTTON_SAVE1_NAME).length(), YFONT_H)) {
			//yc
			yYu();
		}
		else if (yJudgeIn(x, y, YBUTTON_SAVE2_X, YBUTTON_SAVE2_Y, YFONT_W*string(YBUTTON_SAVE2_NAME).length(), YFONT_H)) {
			//处理图
			yPr();
		}
		else if (yJudgeIn(x, y, YBUTTON_SUB_X, YBUTTON_SUB_Y, YFONT_W*string(YBUTTON_SUB_NAME).length(), YFONT_H)) {
			flag_sub = (flag_sub + 1) % 2;
		}
		break;
	default:
		break;
	}
}
//判断是一个缺口还是两个缺口
void yJudge(list<double> &li) {
	
}
//找外环 & 中心
void yFun1(Mat &imggray, int &px, int &py) {
	double minv = 255 * imggray.cols*imggray.rows;
	int dd = YDDD;
	int ss = YSSS;
	Mat img3(YEDGE, YEDGE, CV_8UC1, Scalar(255));
	list<double> li_final;
	//定范围，i = x0;i<x1;i=i+dd j
	for (int i = 0; i < imggray.cols - img3.cols + 1; i = i + dd) {
		for (int j = 0; j < imggray.rows - img3.rows + 1; j = j + dd) {
			Mat imageROI = imggray(Rect(i, j, YEDGE, YEDGE));
			Mat img2(YEDGE, YEDGE, CV_8UC1, Scalar(255));//固定的全环
			circle(img2, Point2i(YEDGE / 2, YEDGE / 2), YEDGE / 2, Scalar(0));
			circle(img2, Point2i(YEDGE / 2, YEDGE / 2), (YEDG12 - YEDG11) / 2, Scalar(0));
			floodFill(img2, Point2i(YEDG11 - 5, YEDGE / 2), 0);
			Mat img;
			Mat imgr;
			resize(imageROI, imgr, Size(ss, ss));
			//存到一个List中，根据阈值划分
			list<double> li;
			int flag = 0;
			for (int k = -3; k <= 4; k++) {
				img2.copyTo(img);
				yRot(1, img, k * 45);
				resize(img, img, Size(ss, ss));
				img = ~img;
				Mat imgand = img & imgr;
				double sum = mean(imgand)[0];
				li.push_back(sum);
				if (sum < minv) {
					flag = 1;
					minv = sum;
					px = i;
					py = j;
					ang[0] = k * 45;
					cout << minv << " " << i << " " << j << " " << ang[0] << endl;
				}
			}
			if (flag == 1) {
				li_final = li;
			}
		}
		cout << (double)(i * 100) / (imggray.cols - img3.cols + 1) << "%: " << endl;
	}
	cout << "100%." << endl;
	//判断li_final
	px += (YEDGE / 2);
	py += (YEDGE / 2);
	return;
}
//找内三环
void yFun2(Mat &imgROI, int num) {
	//内三环
	double minv = 255 * imgROI.cols*imgROI.rows;
	Mat img2(YEDGE, YEDGE, CV_8UC1, Scalar(255));//固定的全环
	if (num == 2) {
		circle(img2, Point2i(YEDGE / 2, YEDGE / 2), (YEDG22 - YEDG21) / 2, Scalar(0));
		circle(img2, Point2i(YEDGE / 2, YEDGE / 2), (YEDG32 - YEDG31) / 2, Scalar(0));
		floodFill(img2, Point2i(YEDG31 - 5, YEDGE / 2), 0);
	}
	else if (num == 3) {
		circle(img2, Point2i(YEDGE / 2, YEDGE / 2), (YEDG42 - YEDG41) / 2, Scalar(0));
		circle(img2, Point2i(YEDGE / 2, YEDGE / 2), (YEDG52 - YEDG51) / 2, Scalar(0));
		floodFill(img2, Point2i(YEDG51 - 5, YEDGE / 2), 0);
	}
	else {
		circle(img2, Point2i(YEDGE / 2, YEDGE / 2), (YEDG62 - YEDG61) / 2, Scalar(0));
		circle(img2, Point2i(YEDGE / 2, YEDGE / 2), (YEDG72 - YEDG71) / 2, Scalar(0));
		floodFill(img2, Point2i(YEDG71 - 5, YEDGE / 2), 0);
	}
	Mat img;
	//存到一个List中，根据阈值划分
	list<double> li;
	for (int k = -3; k <= 4; k++) {
		img2.copyTo(img);
		yRot(num, img, k * 45);
		img = ~img;
		Mat imgand = img & imgROI;
		double sum = mean(imgand)[0];
		li.push_back(sum);
		if (sum < minv) {//...//
			minv = sum;
			ang[num - 1] = k * 45;
		}
	}
	li.sort();
}
int main() {
	namedWindow(YWIN_NAME, CV_WINDOW_NORMAL);
	namedWindow(YDWIN_NAME, CV_WINDOW_NORMAL);
	setMouseCallback(YWIN_NAME, on_MouseHandle, (void*)&winimg);
	int error = 0;
	srand((unsigned int)time(NULL));
	for (int i = 0; i < 4; i++) {
		ang[i] = 45 * (rand() % 8 - 3);
		rang[i] = 45 * (rand() % 8 - 3);
		int ang[4] = { 0 };//最外环的角度（-135~180）到最内环
		int rang[4] = { 0 };
	}
	while (1) {
		//实时采集并显示
		if (flagstart == 0) {
			//读取
			Mat frame;
			capture >> frame;
			if (frame.empty()) {
				error++;
				if (error > 10) {
					exit(0);
				}
				cout << "can not load the frame" << endl;
				continue;
			}
			imshow(YDWIN_NAME, frame);
			frame = frame(Rect((YDEFPIC_W - YDEFPIC_H) / 2, 0, YIMG_GRAY_W, YIMG_GRAY_H));
			cvtColor(frame, imggray, CV_BGR2GRAY);
			imggray.copyTo(dir);
		}
		//绘制按钮界面
		yDrawButLab();
		//确定外环及中心
		if (flagh1 == 0 && flagstart == 1) {
			yFun1(imggray, px, py);
			flagh1 = 1;
			//确定内三环
			Mat imageROI = imggray(Rect(px - (YEDGE / 2), py - (YEDGE / 2), YEDGE, YEDGE));
			for (int i = 2; i <= 4; i++) {
				yFun2(imageROI, i);
			}
			//减少影响
			li.clear();
			for (int i = 0; i < 4; i++) {
				li.push_back(ang[i]);
			}
			li.unique();
			//
			cout << li.size() << " ";
			for (list<int>::iterator it = li.begin(); it != li.end(); it++) {
				cout << (*it) / 45;
			}
			cout << " ";
			for (int i = 0; i < 4; i++) {
				cout << ang[i] / 45;
			}
			cout << endl;
			//
			list<int>::iterator it = li.begin();
			for (unsigned int i = 0; i < 4; i++) {
				if (i < li.size()) {
					rang[i] = *it;
					it++;
				}
				else {
					rang[i] = ang[i];
				}
				cout << rang[i] / 45 << "   ";//
			}
			cout << endl;//
		}
		//绘制灰度处理图
		{
			Mat imageROI = winimg(Rect(YIMG_GRAY_X, YIMG_GRAY_Y, YIMG_GRAY_W, YIMG_GRAY_H));
			split(imageROI, channels);
			for (int i = 0; i < 3; i++) {
				imggray.copyTo(channels.at(i));
			}
			Mat imggrayc3;
			merge(channels, imggrayc3);
			if (flag_sub == YIS_SHOW) {
				//绘制辅助
				line(imggrayc3, Point2i(px - YDL, py), Point2i(px + YDL, py), Scalar(0, 0, 255), 3);
				line(imggrayc3, Point2i(px, py - YDL), Point2i(px, py + YDL), Scalar(255, 0, 0), 3);
			}
			imggrayc3.copyTo(imageROI);
		}
		//显示
		imshow(YWIN_NAME, winimg);
		if (waitKey(10) == '\r') {
			exit(0);
		}
		for (int i = 0; i < 4; i++) {
			//	cout << rang[i] << "   ";
		}
		//cout << endl;//
	}
	return 0;
}
