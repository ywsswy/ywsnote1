#include <opencv2/opencv.hpp>
#include <iostream>
using namespace std;
using namespace cv;
class YNumRec
{
public:
	YNumRec(int type){
		switch (type){
		case 1://ys100
			numnum = 4;
			g_x = 128;
			g_y = 188;
			g_height = 78;
			g_width = 204;//正好包住四个数字，三个空格
			filename = "YS-100.jpg";
			totalratio = 1.0;
			buf1 = 11.625;
			numratio = totalratio*(buf1) / (numnum*buf1 + (numnum - 1));
			spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
			break;
		case 2://test1
			numnum = 4;
			g_x = 1466;
			g_y = 909;
			g_height = 250;
			g_width = 484;//正好包住四个数字，三个空格
			filename = "2.jpg";
			totalratio = 1.0;
			buf1 = 11.625;
			numratio = totalratio*(buf1) / (numnum*buf1 + (numnum - 1));
			spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
			break;
		case 3://test2
			numnum = 4;
			g_x = 1581;
			g_y = 908;
			g_height = 227;
			g_width = 491;
			filename = "2.jpg";
			totalratio = 1.0;
			buf1 = 11.625;
			numratio = totalratio*(buf1) / (numnum*buf1 + (numnum - 1));
			spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
			break;
		case 4://test3
			numnum = 1;
			g_x = 319;
			g_y = 139;
			g_height = 119;
			g_width = 57;
			filename = "3.jpg";
			totalratio = 1.0;
			buf1 = 11.625;
			numratio = totalratio*(buf1) / (numnum*buf1 + (numnum - 1));
			spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
			break;
		case 5://test4
			numnum = 5;
			g_x = 750 - 305;
			g_y = 133;
			g_height = 121;
			g_width = 305;
			filename = "3.jpg";
			totalratio = 1.0;
			buf1 = 11.625;
			numratio = totalratio*(buf1) / (numnum*buf1 + (numnum - 1));
			spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
			break;
		case 6://公司计算器模版
			numnum = 11 + 1;
			g_x = 164 - 87;
			g_y = 176;
			g_width = 1033 + 87;
			g_height = 176;
			filename = "mod1.jpg";
			totalratio = 1.0;
			buf1 = 11.625;
			numratio = totalratio*(buf1) / (numnum*buf1 + (numnum - 1));//11.625*11+10 = 1033//7.492293744333636
			spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
			b_width = g_width*1.089285714285714;//w2/w
			b_height = g_height*1.704545454545455;//h2/h
			b_x = g_x*(1 - (77 - 28) / 1120);//w3/w = (g_x-b_x)/w
			b_y = g_y*(1 - (176 - 82) / 176);
			break; 
		case 7://公司计算器模版
			numnum = 11 + 1;
			g_x = 164 - 87;
			g_y = 176;
			g_width = 1033 + 87;
			g_height = 176;
			filename = "mod1.jpg";
			totalratio = 1.0;
			buf1 = 11.625;
			numratio = totalratio*(buf1) / (numnum*buf1 + (numnum - 1));//11.625*11+10 = 1033//7.492293744333636
			spaceratio = (numnum > 1) ? (totalratio - numnum * numratio) / (numnum - 1) : 0.0;
			b_width = g_width*1.089285714285714;//w2/w
			b_height = g_height*1.704545454545455;//h2/h
			b_x = g_x*(1 - (77 - 28) / 1120);//w3/w = (g_x-b_x)/w
			b_y = g_y*(1 - (176 - 82) / 176);
			break;
		}
	}
	int numnum;
	float totalratio;
	float numratio;
	float spaceratio;
	int g_x;
	int g_y;
	int g_height;
	int g_width;//正好包住四个数字，三个空格
	char *filename;
	float buf1;
	int b_x;
	int b_y;
	int b_height;
	int b_width;	
};
enum YYNUM{
	YY1 = 36,
	YY2 = 93,
	YY3 = 109,
	YY4 = 46,
	YY5 = 107,
	YY6 = 123,
	YY7 = 39,
	YY8 = 127,
	YY9 = 111,
	YY0 = 119
};
int main(){
	// 1
	//2 3
	// 4
	//5 6
	// 7
	/*
	cout << (1 << 2) + (1 << 5) << endl;//1
	cout << (1 << 0) + (1 << 2) + (1 << 3) + (1 << 4) + (1 << 6) << endl;//2
	cout << (1 << 0) + (1 << 2) + (1 << 3) + (1 << 5) + (1 << 6) << endl;
	cout << (1 << 1) + (1 << 3) + (1 << 2) + (1 << 5) << endl;
	cout << (1 << 0) + (1 << 1) + (1 << 3) + (1 << 5) + (1 << 6) << endl;//5
	cout << (1 << 0) + (1 << 1) + (1 << 3) + (1 << 4) + (1 << 5) + (1 << 6) << endl;
	cout << (1 << 0) + (1 << 1) + (1 << 2) + (1 << 5) << endl;//7
	cout << (1 << 0) + (1 << 1) + (1 << 2) + (1 << 3) + (1 << 4) + (1 << 5) + (1 << 6) << endl;
	cout << (1 << 0) + (1 << 1) + (1 << 2) + (1 << 3) + (1 << 5) + (1 << 6) << endl;
	cout << (1 << 0) + (1 << 1) + (1 << 2) + (1 << 4) + (1 << 5) + (1 << 6) << endl;*/
	
	YNumRec ys100(7);
	//
	//Mat src = imread("4.jpg");
	//Mat srccut = src(Rect(28, 8, 245 - 28, 481 - 8));
	//imwrite("4re.jpg", srccut);
	Mat srccut = imread("4re.jpg");//
	Mat imresize;
//	imshow("src", src);
	//resize(srccut, imresize, Size(src.cols*1.05333054106, src.rows));
	resize(srccut, imresize, Size((int)(ys100.g_width*ys100.numratio / ys100.totalratio), ys100.g_height));
	//imshow("imresize", imresize);
	cvtColor(imresize, imresize, CV_BGR2GRAY);
	threshold(imresize, imresize, 20, 255, THRESH_BINARY);
	//imshow("bin", imresize);
	Mat imsrc = imread(ys100.filename);
	cvtColor(imsrc, imsrc, CV_BGR2GRAY);
	Mat imroi = imsrc(Rect(ys100.g_x,ys100.g_y, ys100.g_width, ys100.g_height));//整体正切边界切割
	Mat imroi1;
	Mat imbin;
	Scalar avg = mean(imroi);
	float value = avg[0];
	float resultduan[8] = { 0.0};
	int goodpoint;
	int point;
	// 1
	//2 3
	// 4
	//5 6
	// 7
	for (int k = 0; k < ys100.numnum; k++){
		imroi1 = imroi(Rect(k*ys100.g_width*(ys100.numratio + ys100.spaceratio) / ys100.totalratio, 0, (int)(ys100.g_width*ys100.numratio / ys100.totalratio), ys100.g_height));
		//1
		threshold(imroi1, imbin, value, 255, THRESH_BINARY);
		goodpoint = point = 0;
		for (int i = 0; i < ys100.g_height*0.1346153846153846; i++)
		{
			for (int j = 0; j < (int)(ys100.g_width*ys100.numratio / ys100.totalratio); j++)
			{
				if (imresize.at<uchar>(i, j) == 0)
				{
					point++;
					if (imbin.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[0] = (float)goodpoint / (point + 1);
		//2
		threshold(imroi1, imbin, value, 255, THRESH_BINARY);
		goodpoint = point = 0;
		for (int i = (int)ys100.g_height*0.1346153846153846; i < ys100.g_height*0.4551282051282051; i++)
		{
			for (int j = 0; j < (int)(ys100.g_width*ys100.numratio / ys100.totalratio) / 2; j++)
			{
				if (imresize.at<uchar>(i, j) == 0)
				{
					point++;
					if (imbin.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[1] = (float)goodpoint / (point + 1);
		//3
		threshold(imroi1, imbin, value, 255, THRESH_BINARY);
		goodpoint = point = 0;
		for (int i = (int)ys100.g_height*0.1346153846153846; i < ys100.g_height*0.4551282051282051; i++)
		{
			for (int j = (int)(ys100.g_width*ys100.numratio / ys100.totalratio) / 2; j < (int)(ys100.g_width*ys100.numratio / ys100.totalratio); j++)
			{
				if (imresize.at<uchar>(i, j) == 0)
				{
					point++;
					if (imbin.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[2] = (float)goodpoint / (point + 1);
		//4
		threshold(imroi1, imbin, value, 255, THRESH_BINARY);
		goodpoint = point = 0;
		for (int i = (int)ys100.g_height*0.4551282051282051; i < ys100.g_height*0.5705128205128205; i++)
		{
			for (int j = 0; j < (int)(ys100.g_width*ys100.numratio / ys100.totalratio); j++)
			{
				if (imresize.at<uchar>(i, j) == 0)
				{
					point++;
					if (imbin.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[3] = (float)goodpoint / (point + 1);
		//5
		threshold(imroi1, imbin, value, 255, THRESH_BINARY);
		goodpoint = point = 0;
		for (int i = (int)ys100.g_height*0.5705128205128205; i < ys100.g_height*0.9038461538461538; i++)
		{
			for (int j = 0; j < (int)(ys100.g_width*ys100.numratio / ys100.totalratio) / 2; j++)
			{
				if (imresize.at<uchar>(i, j) == 0)
				{
					point++;
					if (imbin.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[4] = (float)goodpoint / (point + 1);
		//6
		threshold(imroi1, imbin, value, 255, THRESH_BINARY);
		goodpoint = point = 0;
		for (int i = (int)ys100.g_height*0.5705128205128205; i < ys100.g_height*0.9038461538461538; i++)
		{
			for (int j = (int)(ys100.g_width*ys100.numratio / ys100.totalratio) / 2; j < (int)(ys100.g_width*ys100.numratio / ys100.totalratio); j++)
			{
				if (imresize.at<uchar>(i, j) == 0)
				{
					point++;
					if (imbin.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[5] = (float)goodpoint / (point + 1);
		//7
		threshold(imroi1, imbin, value, 255, THRESH_BINARY);
		goodpoint = point = 0;
		for (int i = (int)ys100.g_height*0.9038461538461538; i < ys100.g_height; i++)
		{
			for (int j = 0; j < (ys100.g_width*ys100.numratio / ys100.totalratio) / 2; j++)
			{
				if (imresize.at<uchar>(i, j) == 0)
				{
					point++;
					if (imbin.at<uchar>(i, j) == 0){
						goodpoint++;
					}
				}
			}
		}
		resultduan[6] = (float)goodpoint / (point + 1);
		float sortthresh = 0.0;
		int sortflag = -1;
		while (sortflag == -1){
			int sortvalue = 0;
			for (int j = 0; j < 7; j++){
				if (resultduan[j]>0.5 - sortthresh){
					sortvalue += (1 << j);
					//cout << sortvalue << endl;
				}
			}
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
		cout << sortflag << endl;
	}
	return 0;
}
