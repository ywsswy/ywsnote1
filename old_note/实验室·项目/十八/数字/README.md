#include <opencv2/opencv.hpp>
#include <iostream>
//#define YREGTEST
using namespace std;
using namespace cv;
void yRegister(Mat &imgrgb, Mat &fimgrgb, int &dx, int&dy, int type = -1, Point &plt = Point(0, 0), Point &prb = Point(0, 0));//总对齐
void yRegisterRough(Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul);//粗对齐
void yRegisterThorough(Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul);//细对齐
double yNum(Mat &img, int yp);//识别示数
//都不满足段选时的取舍
//透视变换的差值
//不同透视平面的变换计算
//小数点的腐蚀核
//参数的设定工具完善（黑色缺失）
//更多图片的测试
//定四点工具可上下拉伸
//制作模版图后对模版图进行各种参数确定（边框的框定、数字部分的框定、各比例的计算）
//利用自建工具对定点图片进行透视变换矩阵的计算并保存参数文件
class YNumRec
{
public:
	//从外部获取的参数
	string registername;//对齐使用的[模板]图片路径
	string modelname;//端正标准的[模板]数码管图片路径
	int g_x;//正好包住时的横坐标
	int g_y;//正好包住时的纵坐标
	int g_x2;//右下角
	int g_y2;
	int g_height;//正好包住
	int g_width;//正好包住所有数字的矩形
	int type;//该识别点对应的仪表类型号
	int numnum;//该类型数字位数
	int blockSize;//////////////////////////////////////////////////////
	int constValue;
	int elementSize;
	int blockSize2;
	int constValue2;
	//暂定及内部计算的参数参数
	double bufnum;//一个数字的宽度/一个空格的宽度之商
	double totalratio;//总宽度比例，取1.0
	double numratio;//在总宽度比例下的一个数字比例
	double spaceratio;//在总宽度比例下的一个空格比例
public:
	YNumRec(FileStorage &fs, int &yp, Mat &MArray) {
		string preyp = "yp";
		preyp.append(to_string(yp)).append("_");
		fs[preyp + "path"] >> registername;
		fs[preyp + "path2"] >> modelname;
		fs[preyp + "type"] >> type;
		fs[preyp + "p11"] >> MArray.at<double>(0, 0);
		fs[preyp + "p12"] >> MArray.at<double>(0, 1);
		fs[preyp + "p13"] >> MArray.at<double>(0, 2);
		fs[preyp + "p21"] >> MArray.at<double>(1, 0);
		fs[preyp + "p22"] >> MArray.at<double>(1, 1);
		fs[preyp + "p23"] >> MArray.at<double>(1, 2);
		fs[preyp + "p31"] >> MArray.at<double>(2, 0);
		fs[preyp + "p32"] >> MArray.at<double>(2, 1);
		fs[preyp + "p33"] >> MArray.at<double>(2, 2);
		fs[preyp + "blockSize"] >> blockSize;
		fs[preyp + "constValue"] >> constValue;
		fs[preyp + "elementSize"] >> elementSize;
		fs[preyp + "blockSize2"] >> blockSize2;
		fs[preyp + "constValue2"] >> constValue2;
		string preyt = "yt";
		preyt.append(to_string(type)).append("_");
		fs[preyt + "num"] >> numnum;
		fs[preyt + "xb1"] >> g_x;
		fs[preyt + "yb1"] >> g_y;
		fs[preyt + "xb2"] >> g_x2;
		fs[preyt + "yb2"] >> g_y2;
		fs.release();
		g_width = g_x2 - g_x;
		g_height = g_y2 - g_y;
		//暂定参数及内部计算
		totalratio = 1.0;
		bufnum = 5.625;///////////////////////////////////////////////////
		numratio = totalratio*(bufnum) / (numnum*bufnum + (numnum - 1));
		spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
	}
};
//不同数字对应的数码管段选表
enum YYNUM {
	YY1 = 36,//4+32
	YY2 = 93,
	YY3 = 109,
	YY4 = 46,
	YY5 = 107,
	YY6 = 123,
	YY7 = 37,//1+4+32
	YY8 = 127,
	YY9 = 111,//127-16
	YY0 = 119//127-8
			 // 0
			 //1 2
			 // 3
			 //4 5
			 // 6
};
//输入 测试图片img 识别点yp
//输出 识别出的读数
double yNum(Mat &img, int yp){
	double re = 0;
	//读取模版及初始化相关参数
	FileStorage yfs("ymain.yaml", FileStorage::READ);
	Mat MArray(3, 3, CV_64F, Scalar(0));
	YNumRec ys100(yfs, yp, MArray);
	Mat srcmod = imread(ys100.modelname);
	cvtColor(srcmod, srcmod, CV_BGR2GRAY);
	threshold(srcmod, srcmod, 20, 255, THRESH_BINARY);
	int buf_width = srcmod.cols*ys100.numnum + (ys100.numnum - 1)*srcmod.cols / ys100.bufnum;
	int img_height = img.rows;
	int img_width = img.cols;
	//透视变换
	vector<Point2f> points(img_height*img_width), points_trans(img_height*img_width);
	for (int i = 0; i < img_height; i++) {
		for (int j = 0; j < img_width; j++) {
			points[i*img_width + j] = Point2f(j, i);
		}
	}
	perspectiveTransform(points, points_trans, MArray);
	Mat img_trans = Mat::zeros(img_height, img_width, CV_8UC3);
	int count = 0;
	for (int i = 0; i < img_height; i++) {
		uchar* p = img.ptr<uchar>(i);
		for (int j = 0; j < img_width; j++) {
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
	//对齐
	int dx, dy;
	Mat registerimg = imread(ys100.registername);
	yRegister(img_trans, registerimg, dx, dy);
	ys100.g_x += dx;
	ys100.g_y += dy;
	Mat imsrc;
	cvtColor(img_trans, imsrc, CV_BGR2GRAY);
	//识别小数点位置
	IplImage* src;
	IplImage* gray;
	IplImage* dst;
	//截取小数点区域
	Mat msrc = imsrc(Rect(ys100.g_x, ys100.g_y + 2 * ys100.g_height / 3, ys100.g_width*(ys100.numnum + 1) / ys100.numnum, ys100.g_height * 2 / 3));
	Mat mgray;
	resize(msrc, mgray, Size(buf_width*(ys100.numnum + 1) / ys100.numnum, srcmod.rows * 2 / 3));
	Mat mbin;
	adaptiveThreshold(mgray, mbin, 255, CV_ADAPTIVE_THRESH_MEAN_C, CV_THRESH_BINARY_INV, ys100.blockSize, ys100.constValue);
	Mat element = getStructuringElement(MORPH_RECT, Size(ys100.elementSize, ys100.elementSize));////////////////////////////////////////////////////
	erode(mbin, mbin, element);
	dst = cvCreateImage(cvGetSize(&(IplImage)mbin), 8, 3);
	CvMemStorage* storage = cvCreateMemStorage(0);
	CvSeq* contour = 0;
	IplImage *bin = &(IplImage)mbin;
#ifdef YREGTEST
	CvScalar color;
	cvShowImage("Bin", bin);
	CvFont font;
	cvInitFont(&font, CV_FONT_HERSHEY_SIMPLEX, 0.4, 0.4, 0);
	color = CV_RGB(rand() & 255, rand() & 255, rand() & 255);
	char str[100] = "";
	char buf[100] = "";
#endif
	cvFindContours(bin, storage, &contour, sizeof(CvContour), CV_RETR_CCOMP, CV_CHAIN_APPROX_SIMPLE);//提取轮廓 
	cvZero(dst);
	count = 0;
	int dotloc = 0;
	for (; contour != 0; contour = contour->h_next)
	{
		double tmparea = fabs(cvContourArea(contour));//算面积
		CvRect aRect = cvBoundingRect(contour, 0);//算坐标与长宽
												  //为消除过小连通域对画面产生的混乱感
		if (aRect.height > 100 ||
			aRect.width > 100 ||
			aRect.height < 20 ||
			aRect.width < 20 ||
			tmparea < 600 ||
			aRect.y + aRect.height + 3>mbin.rows ||
			aRect.y - 3 < 0 ||
			aRect.x + aRect.width + 3 > mbin.cols ||
			aRect.x - 3 < 0
			) {
			cvSeqRemove(contour, 0); //删除某轮廓  
			continue;
		}
		count++;
		//获取小数点在第几个数字后面
		dotloc = static_cast<int>((aRect.x + srcmod.cols / 2) / srcmod.cols);
#ifdef YREGTEST
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
		cvPutText(dst, str, cv::Point(aRect.x, aRect.y), &font, color);
#endif
	}
#ifdef YREGTEST
	strcpy(str, "The total number of contours is: ");
	itoa(count, buf, 10);
	strcat(str, buf);
	color = CV_RGB((125 + rand()) + 15 & 255, (125 + rand()) & 255, (125 + rand()) & 255);//尽量偏亮的随机色
	cvPutText(dst, str, cvPoint(0, 120), &font, color);
	cvShowImage("Components", dst);
	waitKey();
#endif
	//识别数字
	//整体正切边界切割
	Mat imroi = imsrc(Rect(ys100.g_x, ys100.g_y, ys100.g_width, ys100.g_height));
	//判断越界//类型转换
	Mat imresize;
	resize(imroi, imresize, Size(buf_width, srcmod.rows));
	Mat imbin;
	adaptiveThreshold(imresize, imbin, 255, CV_ADAPTIVE_THRESH_MEAN_C, CV_THRESH_BINARY, ys100.blockSize2, ys100.constValue2);
	Mat imroi1;
	double resultduan[8] = { 0.0 };
	int goodpoint;
	int point;
	//各数字依次模版匹配
	for (int k = 0; k < ys100.numnum; k++) {
		imroi1 = imbin(Rect(static_cast<int>(k*buf_width*(ys100.numratio + ys100.spaceratio) / ys100.totalratio), 0, srcmod.cols, srcmod.rows));
		//1
		goodpoint = point = 0;
		for (int i = 0; i < srcmod.rows*0.1346153846153846; i++) {
			for (int j = 0; j < srcmod.cols; j++) {
				if (srcmod.at<uchar>(i, j) == 0) {
					point++;
					if (imroi1.at<uchar>(i, j) == 0) {
						goodpoint++;
					}
				}
			}
		}
		resultduan[0] = (float)goodpoint / (point + 1);
		//2
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.1346153846153846); i < srcmod.rows*0.4551282051282051; i++) {
			for (int j = 0; j < srcmod.cols / 2; j++) {
				if (srcmod.at<uchar>(i, j) == 0) {
					point++;
					if (imroi1.at<uchar>(i, j) == 0) {
						goodpoint++;
					}
				}
			}
		}
		resultduan[1] = (float)goodpoint / (point + 1);
		//3
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.1346153846153846); i < srcmod.rows*0.4551282051282051; i++) {
			for (int j = srcmod.cols / 2; j < srcmod.cols; j++) {
				if (srcmod.at<uchar>(i, j) == 0) {
					point++;
					if (imroi1.at<uchar>(i, j) == 0) {
						goodpoint++;
					}
				}
			}
		}
		resultduan[2] = (float)goodpoint / (point + 1);
		//4
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.4551282051282051); i < srcmod.rows*0.5705128205128205; i++) {
			for (int j = 0; j < srcmod.cols; j++) {
				if (srcmod.at<uchar>(i, j) == 0) {
					point++;
					if (imroi1.at<uchar>(i, j) == 0) {
						goodpoint++;
					}
				}
			}
		}
		resultduan[3] = (float)goodpoint / (point + 1);
		//5
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.5705128205128205); i < srcmod.rows*0.9038461538461538; i++) {
			for (int j = 0; j < srcmod.cols / 2; j++) {
				if (srcmod.at<uchar>(i, j) == 0) {
					point++;
					if (imroi1.at<uchar>(i, j) == 0) {
						goodpoint++;
					}
				}
			}
		}
		resultduan[4] = (float)goodpoint / (point + 1);
		//6
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.5705128205128205); i < srcmod.rows*0.9038461538461538; i++) {
			for (int j = srcmod.cols / 2; j < srcmod.cols; j++) {
				if (srcmod.at<uchar>(i, j) == 0) {
					point++;
					if (imroi1.at<uchar>(i, j) == 0) {
						goodpoint++;
					}
				}
			}
		}
		resultduan[5] = (float)goodpoint / (point + 1);
		//7
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.9038461538461538); i < srcmod.rows; i++) {
			for (int j = 0; j < srcmod.cols; j++) {
				if (srcmod.at<uchar>(i, j) == 0) {
					point++;
					if (imroi1.at<uchar>(i, j) == 0) {
						goodpoint++;
					}
				}
			}
		}
		resultduan[6] = (float)goodpoint / (point + 1);
		//不断降低要求，使得识别出数字
		double sortthresh = 0.0;
		int sortflag = -1;
		while (sortflag == -1) {
			int sortvalue = 0;
			for (int j = 0; j < 7; j++) {
				//	cout << resultduan[j] << endl;
				if (resultduan[j]>0.5 - sortthresh) {
					sortvalue += (1 << j);
					//cout << sortvalue << endl;
				}
			}
			//cout << endl;
			//cout << sortvalue << endl;
			if (sortvalue == YY0) {
				sortflag = 0;
			}
			else if (sortvalue == YY1) {
				sortflag = 1;
			}
			else if (sortvalue == YY2) {
				sortflag = 2;
			}
			else if (sortvalue == YY3) {
				sortflag = 3;
			}
			else if (sortvalue == YY4) {
				sortflag = 4;
			}
			else if (sortvalue == YY5) {
				sortflag = 5;
			}
			else if (sortvalue == YY6) {
				sortflag = 6;
			}
			else if (sortvalue == YY7) {
				sortflag = 7;
			}
			else if (sortvalue == YY8) {
				sortflag = 8;
			}
			else if (sortvalue == YY9) {
				sortflag = 9;
			}
			sortthresh += 0.1;
		}
		re = re * 10;
		re = re + sortflag;
	}
	return re / pow(10, (ys100.numnum - dotloc));
}
//细对齐
void yRegisterThorough(Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul) {
	Mat imgA = fimggray(Rect(fimggray.cols / 3, fimggray.rows / 3, fimggray.cols / 3, fimggray.rows / 3));//模版图的中间1/3的感兴趣区域
	double fmean = mean(imgA)[0];//模版图中心1/3区域的平均“亮度”
	Mat imgB;//测试图的裁剪区域
	double xmean;//测试图裁剪区域的平均“亮度”
	int foundflag = 0;//0 还没有计算差值；1 已经计算了一次差值
	double xvalue;//减弱了测试图与模版图的比较区域平均“亮度”差异后的匹配差异
	double minvalue;//减弱了测试图与模版图的比较区域最匹配时平均“亮度”差异后的最匹配差异
	int minx, miny;//减弱了测试图与模版图的比较区域最匹配时平均“亮度”差异后的最匹配差异情况下的测试图区域左上角横纵坐标
	for (int i = fimggray.rows / 3 + (dy - 1)*mul; i < fimggray.rows / 3 + (dy + 1)*mul; i++) {//考虑了粗对齐存在的误差
		for (int j = fimggray.cols / 3 + (dx - 1)*mul; j < fimggray.cols / 3 + (dx + 1)*mul; j++) {
			if (j + fimggray.cols / 3>fimggray.cols || i + fimggray.rows / 3>fimggray.rows)//越界检查
				continue;
			imgB = imggray(Rect(j, i, fimggray.cols / 3, fimggray.rows / 3));
			xmean = mean(imgB)[0];
			//减去“亮度”差异后计算匹配差异
			if (xmean < fmean) {
				Mat imgC(imgA.rows, imgA.cols, CV_8UC1, Scalar(static_cast<uchar>(fmean - xmean)));
				xvalue = mean(imgB + imgC - imgA)[0] + mean(imgA - imgB - imgC)[0];
			}
			else {
				Mat imgC(imgA.rows, imgA.cols, CV_8UC1, Scalar(static_cast<uchar>(xmean - fmean)));
				xvalue = mean(imgB - imgC - imgA)[0] + mean(imgA + imgC - imgB)[0];
			}
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
//粗对齐
void yRegisterRough(Mat &imggray, Mat &fimggray, int &dx, int &dy, int mul) {
	Mat imgA = fimggray(Rect(fimggray.cols / 3, fimggray.rows / 3, fimggray.cols / 3, fimggray.rows / 3));//模版图的中间1/3的感兴趣区域
	double fmean = mean(imgA)[0];//模版图中心1/3区域的平均“亮度”
	Mat imgB;//测试图的裁剪区域
	double xmean;//测试图裁剪区域的平均“亮度”
	int foundflag = 0;//0 还没有计算差值；1 已经计算了一次差值
	double xvalue;//减弱了测试图与模版图的比较区域平均“亮度”差异后的匹配差异
	double minvalue;//减弱了测试图与模版图的比较区域最匹配时平均“亮度”差异后的最匹配差异
	int minx, miny;//减弱了测试图与模版图的比较区域最匹配时平均“亮度”差异后的最匹配差异情况下的测试图区域左上角横纵坐标
	for (int i = 0; i < fimggray.rows * 2 / 3; i++) {
		for (int j = 0; j < fimggray.cols * 2 / 3; j++) {
			if (j + fimggray.cols / 3>fimggray.cols || i + fimggray.rows / 3>fimggray.rows)//越界检查
				continue;
			imgB = imggray(Rect(j, i, fimggray.cols / 3, fimggray.rows / 3));
			xmean = mean(imgB)[0];
			//减去“亮度”差异后计算匹配差异
			if (xmean < fmean) {
				Mat imgC(imgA.rows, imgA.cols, CV_8UC1, Scalar(static_cast<uchar>(fmean - xmean)));
				xvalue = mean(imgB + imgC - imgA)[0] + mean(imgA - imgB - imgC)[0];
			}
			else {
				Mat imgC(imgA.rows, imgA.cols, CV_8UC1, Scalar(static_cast<uchar>(xmean - fmean)));
				xvalue = mean(imgB - imgC - imgA)[0] + mean(imgA + imgC - imgB)[0];
			}
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
void yRegister(Mat &imgrgb, Mat &fimgrgb, int &dx, int&dy, int type/* = -1*/, Point &plt/* = Point(0, 0)*/, Point &prb/* = Point(0, 0)*/) {
	if (type == -1) {
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
	else {
		//待补充有忽视干扰区域的情况
		exit(0);
	}
}
int main() {
#if 1
	string imgrgbstr = R"(ypic\2017-05-11\0.279.jpg)";
#endif
	Mat imgrgb = imread(imgrgbstr);
	double re = yNum(imgrgb, 1);
	cout << re << endl;
	return 0;
}
