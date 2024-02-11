//四点均衡判断
//都不满足时的取舍
//参数：
//小数点的腐蚀核
#include <opencv2/opencv.hpp>
#include <iostream>
using namespace std;
using namespace cv;
#define TESTNUM 2
class YNumRec
{
public:
	YNumRec(int type){
		switch (type){
		case 1://公司计算器模版
			//现在假定读取的12个参数如下
			modelname = "4re.jpg";
			filename = "t2.jpg";
			numnum = 12;
			b_x = 28;
			b_y = 76;
			b_width = 1222;
			b_height = 290;
			bufw = 1.089285714285714;
			bufh = 1.704545454545455;
			bufx = 25.52083333333333;
			bufy = 3.136842105263158;
			/*x:28
			y:76
			width:1222
			height:290
			*/
			//转换计算
			g_x = b_x + b_width / bufx;
			g_y = b_y + b_height / bufy;
			g_width = static_cast<int>(b_width / bufw);
			g_height = static_cast<int>(b_height / bufh);
			break;
		case 2://黑白压力表
			//现在假定读取的12个参数如下
			modelname = "4re.jpg";
			filename = "YS-101.jpg";// "YS-100.jpg";
			numnum = 4;
			totalratio = 1.0;
			bufnum = 5.625;
			b_x = 117;
			b_y = 169;
			b_width = 448 - b_x;
			b_height = 285 - b_y;
			bufw = static_cast<double>(b_width) / (331 - 131);
			bufh = static_cast<double>(b_height) / (265 - 190);
			bufx = static_cast<double>(b_width) / (131 - 117);
			bufy = static_cast<double>(b_height) / (190 - 169);
			//转换计算
			g_x = b_x + b_width / bufx;
			g_y = b_y + b_height / bufy;
			g_width = static_cast<int>(b_width / bufw);
			g_height = static_cast<int>(b_height / bufh);
			numratio = totalratio*(bufnum) / (numnum*bufnum + (numnum - 1));
			spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
			break;
		}
	}
	//从外部获取的参数
	char *modelname;//端正标准的[模板]数码管图片路径
	char *filename;//放射变换标准后的待识别的[测试图]路径
	int numnum;//横向数字个数
	double bufnum;//一个数字的宽度/一个空格的宽度之商
	int b_x;//表盘（让人容易截取的有棱有角的部分）的横坐标
	int b_y;//表盘的纵坐标
	int b_height;//表盘的宽
	int b_width;//表盘的高
	double bufw;//棱角框的宽/正好包住数字的宽之商
	double bufh;//棱角框的高/正好包住数字的高之商
	double bufx;//棱角框的宽/左楞到正好包住数字部分的距离之商
	double bufy;//棱角框的高/上楞到正好包住数字部分的距离之商
	//内部计算的参数
	double totalratio;//总宽度比例，取1.0
	double numratio;//在总宽度比例下的一个数字比例
	double spaceratio;//在总宽度比例下的一个空格比例
	int g_x;//正好包住时的横坐标
	int g_y;//正好包住时的纵坐标
	int g_height;//正好包住
	int g_width;//正好包住所有数字的矩形
};
//不同数字对应的数码管段选表
enum YYNUM{
	YY1 = 36,//4+32
	YY2 = 93,
	YY3 = 109,
	YY4 = 46,
	YY5 = 107,
	YY6 = 123,
	YY7 = 39,
	YY8 = 127,
	YY9 = 111,//127-16
	YY0 = 119//127-8
	// 0
	//1 2
	// 3
	//4 5
	// 6
};
void yNum(){
	//读取模版及相关参数
	YNumRec ys100(TESTNUM);
	Mat srcmod = imread(ys100.modelname);
	cvtColor(srcmod, srcmod, CV_BGR2GRAY);
	threshold(srcmod, srcmod, 20, 255, THRESH_BINARY);
	//读取测试图 设置相关参数
	Mat imsrc = imread(ys100.filename);
	int buf_width = srcmod.cols*ys100.numnum + (ys100.numnum - 1)*srcmod.cols / ys100.bufnum;
	int blockSize = 95;//
	int constValue = 0;
	//读取放射变换参数，并变换
	FILE*fp = fopen("D:\\tmp\\hh2.txt", "r");
	Mat MArray(2, 3, CV_64F, Scalar(0));
	fscanf(fp, "%lf%lf%lf%lf%lf%lf", &MArray.at<double>(0, 0), &MArray.at<double>(0, 1), &MArray.at<double>(0, 2), &MArray.at<double>(1, 0), &MArray.at<double>(1, 1), &MArray.at<double>(1, 2));
	fclose(fp);
	warpAffine(imsrc, imsrc, MArray, Size(imsrc.cols, imsrc.rows), 1, 0, Scalar(0));
	cvtColor(imsrc, imsrc, CV_BGR2GRAY);
	//对齐
	//...
	//识别小数点位置
	IplImage* src;
	IplImage* gray;
	IplImage* dst;
	//截取小数点区域
	Mat msrc = imsrc(Rect(ys100.g_x, ys100.g_y + 2 * ys100.g_height / 3, ys100.g_width*(ys100.numnum + 1) / ys100.numnum, ys100.g_height * 2 / 3));
	Mat mgray;
	resize(msrc, mgray, Size(buf_width*(ys100.numnum + 1) / ys100.numnum, srcmod.rows * 2 / 3));
	Mat mbin;
	adaptiveThreshold(mgray, mbin, 255, CV_ADAPTIVE_THRESH_MEAN_C, CV_THRESH_BINARY_INV, blockSize, constValue);
	Mat element = getStructuringElement(MORPH_RECT, Size(3, 3));//
	erode(mbin, mbin, element);
	dst = cvCreateImage(cvGetSize(&(IplImage)mbin), 8, 3);
	CvMemStorage* storage = cvCreateMemStorage(0);
	CvSeq* contour = 0;
	CvScalar color;
	IplImage *bin = &(IplImage)mbin;
	cvShowImage("Bin", bin);
	cvFindContours(bin, storage, &contour, sizeof(CvContour), CV_RETR_CCOMP, CV_CHAIN_APPROX_SIMPLE);//提取轮廓 
	cvZero(dst);//清空数组     
	char str[100] = "";
	char buf[100] = "";
	CvFont font;
	cvInitFont(&font, CV_FONT_HERSHEY_SIMPLEX, 0.4, 0.4, 0);
	color = CV_RGB(rand() & 255, rand() & 255, rand() & 255);//创建一个色彩值 
	int count = 0;
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
			tmparea < 500 ||
			aRect.y + aRect.height + 3>mbin.rows ||
			aRect.y - 3 < 0 ||
			aRect.x + aRect.width + 3 > mbin.cols ||
			aRect.x - 3 < 0
			){
			cvSeqRemove(contour, 0); //删除某轮廓  
			continue;
		}
		count++;
		//获取小数点在哪个数字后面
		dotloc = static_cast<int>((aRect.x + srcmod.cols / 2) / srcmod.cols);
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
	}
	strcpy(str, "The total number of contours is: ");
	itoa(count, buf, 10);
	strcat(str, buf);
	color = CV_RGB((125 + rand()) + 15 & 255, (125 + rand()) & 255, (125 + rand()) & 255);//尽量偏亮的随机色
	cvPutText(dst, str, cvPoint(0, 120), &font, color);
	cvShowImage("Components", dst);
	waitKey();
	//识别数字
	//整体正切边界切割
	Mat imroi = imsrc(Rect(ys100.g_x, ys100.g_y,ys100.g_width,ys100.g_height));
	//判断越界//类型转换
	Mat imresize;
	resize(imroi, imresize, Size(buf_width, srcmod.rows));
	
	Mat imbin;
	adaptiveThreshold(imresize, imbin, 255, CV_ADAPTIVE_THRESH_MEAN_C, CV_THRESH_BINARY, blockSize, constValue);
	
	Mat imroi1;
	double resultduan[8] = { 0.0 };
	int goodpoint;
	int point;
	//各数字依次模版匹配
	for (int k = 0; k < ys100.numnum; k++){
		imroi1 = imbin(Rect(static_cast<int>(k*buf_width*(ys100.numratio + ys100.spaceratio) / ys100.totalratio), 0,srcmod.cols, srcmod.rows));
		//1
		goodpoint = point = 0;
		for (int i = 0; i < srcmod.rows*0.1346153846153846; i++){
			for (int j = 0; j < srcmod.cols; j++){
				if (srcmod.at<uchar>(i, j) == 0){
					point++;
					if (imroi1.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[0] = (float)goodpoint / (point + 1);
		//2
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.1346153846153846); i < srcmod.rows*0.4551282051282051; i++){
			for (int j = 0; j < srcmod.cols/2; j++){
				if (srcmod.at<uchar>(i, j) == 0){
					point++;
					if (imroi1.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[1] = (float)goodpoint / (point + 1);
		//3
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.1346153846153846); i < srcmod.rows*0.4551282051282051; i++){
			for (int j = srcmod.cols / 2; j < srcmod.cols; j++){
				if (srcmod.at<uchar>(i, j) == 0){
					point++;
					if (imroi1.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[2] = (float)goodpoint / (point + 1);
		//4
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.4551282051282051); i < srcmod.rows*0.5705128205128205; i++){
			for (int j = 0; j < srcmod.cols; j++){
				if (srcmod.at<uchar>(i, j) == 0){
					point++;
					if (imroi1.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[3] = (float)goodpoint / (point + 1);
		//5
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.5705128205128205); i < srcmod.rows*0.9038461538461538; i++){
			for (int j = 0; j < srcmod.cols / 2; j++){
				if (srcmod.at<uchar>(i, j) == 0){
					point++;
					if (imroi1.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[4] = (float)goodpoint / (point + 1);
		//6
		goodpoint = point = 0;
		for (int i = static_cast<int>(srcmod.rows*0.5705128205128205); i < srcmod.rows*0.9038461538461538; i++){
			for (int j = srcmod.cols / 2; j < srcmod.cols; j++){
				if (srcmod.at<uchar>(i, j) == 0){
					point++;
					if (imroi1.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[5] = (float)goodpoint / (point + 1);
		//7
		goodpoint = point = 0;
		for (int i = static_cast<int>(ys100.g_height*0.9038461538461538); i < ys100.g_height; i++){
			for (int j = 0; j < srcmod.cols; j++){
				if (srcmod.at<uchar>(i, j) == 0){
					point++;
					if (imroi1.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[6] = (float)goodpoint / (point + 1);
		//不断降低要求，使得识别出数字
		double sortthresh = 0.0;
		int sortflag = -1;
		while (sortflag == -1){
			int sortvalue = 0;
			for (int j = 0; j < 7; j++){
			//	cout << resultduan[j] << endl;
				if (resultduan[j]>0.5 - sortthresh){
					sortvalue += (1 << j);
					//cout << sortvalue << endl;
				}
			}
			//cout << endl;
			//cout << sortvalue << endl;
			if (sortvalue == YY0){
				sortflag = 0;
			}
			else if (sortvalue == YY1){
				sortflag = 1;
			}
			else if (sortvalue == YY2){
				sortflag = 2;
			}
			else if (sortvalue == YY3){
				sortflag = 3;
			}
			else if (sortvalue == YY4){
				sortflag = 4;
			}
			else if (sortvalue == YY5){
				sortflag = 5;
			}
			else if (sortvalue == YY6){
				sortflag = 6;
			}
			else if (sortvalue == YY7){
				sortflag = 7;
			}
			else if (sortvalue == YY8){
				sortflag = 8;
			}
			else if (sortvalue == YY9){
				sortflag = 9;
			}
			sortthresh += 0.1;
		}
		//输出识别的数字
		if (k == dotloc){
			cout << ".";
		}
		cout << sortflag;
	}
	return;
}
int main(){
	yNum();
	return 0;
}
