#include <opencv2/opencv.hpp>  
using namespace cv;
using namespace std;
class CvMouseParam{
public:
	static int event;
	static int x;
	static int y;
	static int flags;
	static int num;
	static int oldnum;
};
class CvWindowName{
public:
	static string src1;
	static string model1;
};
class CvPicName{
public:
	static string pic1;
	static string pic2;
	static string pic3;
	static string pic4;
};
void onMouse(int event, int x, int y, int flags, void* param)
{
	if (event == CV_EVENT_LBUTTONDOWN)
	{
		cout << x << ","<<y<< endl;
		CvMouseParam::x = x;
		CvMouseParam::y = y;
		CvMouseParam::num++;
	}
}
int CvMouseParam::num = 0;
int CvMouseParam::x = x;
int CvMouseParam::y = y;
int CvMouseParam::oldnum = 0;
string CvWindowName::src1 = "SRC1";
string CvWindowName::model1 = "MODEL1";
string CvPicName::pic3 = R"(C:\Users\admin\Desktop\daea38dbb6fd5266874128b5a218972bd50736cd.jpg)";
string CvPicName::pic2 = R"(F:\fc\document\tb\before0305\标准实验图 (2)\标准实验图\数显压力表\model1.jpg)";
string CvPicName::pic4 = R"(F:\fc\document\tb\before0305\标准实验图 (2)\标准实验图\数显压力表\src1.jpg)";
string CvPicName::pic1 = R"(D:\program files (x86)\opencv\projects\TestContents1\TestContents1\z.jpg)";
int main()
{
	Mat img, img2;
	Point2f srcTri[3], dstTri[3];
	namedWindow(CvWindowName::src1,1);
	img = imread(CvPicName::pic1);
	setMouseCallback(CvWindowName::src1, onMouse);
	while (waitKey(10) != 27){
		if (CvMouseParam::oldnum != CvMouseParam::num){
			circle(img, Point(CvMouseParam::x, CvMouseParam::y), 3, Scalar(0, 0, 255), 0);
			CvMouseParam::oldnum = CvMouseParam::num;
			imshow(CvWindowName::src1, img);
			if (CvMouseParam::num > 1){
				if (CvMouseParam::num == 5){
					CvMouseParam::num = CvMouseParam::oldnum = 0;
					break;
				}
				srcTri[CvMouseParam::num - 2].x = CvMouseParam::x;
				srcTri[CvMouseParam::num - 2].y = CvMouseParam::y;
			}
		}
	}
	/*
	namedWindow(CvWindowName::model1, 1);
	setMouseCallback(CvWindowName::model1, onMouse);
	img2 = imread(CvPicName::pic2);
	while (waitKey(10) != 27){
		if (CvMouseParam::oldnum != CvMouseParam::num){
			circle(img2, Point(CvMouseParam::x, CvMouseParam::y), 3, Scalar(0, 0, 255), 0);
			CvMouseParam::oldnum = CvMouseParam::num;
			imshow(CvWindowName::model1, img2);
			if (CvMouseParam::num > 1){
				if (CvMouseParam::num == 5){
					break;
				}
				dstTri[CvMouseParam::num - 2].x = CvMouseParam::x;
				dstTri[CvMouseParam::num - 2].y = CvMouseParam::y;
			}
		}
	}
	*/
	dstTri[0].x = 31;
	dstTri[0].y = 83;
	dstTri[1].x = 31;
	dstTri[1].y = 386;
	dstTri[2].x = 1247;
	dstTri[2].y = 83;
	/*
	31, 83
		31, 386
		1247, 83
		1255, 386
*/
	Mat MArray = getAffineTransform(srcTri, dstTri);
	Mat dstImage;
	warpAffine(img, dstImage, MArray, Size(img.cols,img.rows), 1, 0, Scalar(0));
	namedWindow("【进行仿射变换后】",1);
	imshow("【进行仿射变换后】", dstImage);
	waitKey();
	return 0;
}
