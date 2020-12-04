#include <opencv2/opencv.hpp>
#include <iostream>
#include<fstream> 
using namespace std;
using namespace cv;
/*
void on_MouseHandle(int event, int x, int y, int flags, void* param){
while (1){
}
}
*/
void on_MouseHandle(int event, int x, int y, int flags, void* param);
void DrawRectangle(cv::Mat& img, cv::Rect box);
Rect g_rectangele;
bool g_bDrawingBox = false;
RNG g_rng(12345);
int main(int argc, char*argv[]){
	ofstream log("D:\\out.txt");
	streambuf * oldbuf = cout.rdbuf(log.rdbuf());
	g_rectangele = Rect(-1, -1, 0, 0);
	cout << argv[1] << endl;
	Mat src = imread(argv[1]);
	Mat tempImage;
	namedWindow("YWIN");
	setMouseCallback("YWIN", on_MouseHandle, (void*)&src);
	while (1){
		src.copyTo(tempImage);
		if (g_bDrawingBox) DrawRectangle(tempImage, g_rectangele);
		imshow("YWIN", tempImage);
		if (waitKey(10) == 27) break;
	}
	waitKey();
	return 0;
}
void on_MouseHandle(int event, int x, int y, int flags, void* param){//回调函数  
	Mat& image = *(cv::Mat*) param;
	switch (event){
	case EVENT_MOUSEMOVE:
		if (g_bDrawingBox){
			g_rectangele.width = x - g_rectangele.x;
			g_rectangele.height = y - g_rectangele.y;
		}
		break;
	case EVENT_LBUTTONDOWN:
		g_bDrawingBox = true;
		g_rectangele = Rect(x, y, 0, 0);
		break;
	case EVENT_LBUTTONUP:
		g_bDrawingBox = false;
		/*
		if (g_rectangele.width < 0){
			g_rectangele.x += g_rectangele.width;
			g_rectangele.width *= -1;
		}
		if (g_rectangele.height < 0){
			g_rectangele.y += g_rectangele.height;
			g_rectangele.height *= -1;
		}*/
		cout << "x:" << ((g_rectangele.x - g_rectangele.width < g_rectangele.x + g_rectangele.width) ? (g_rectangele.x - g_rectangele.width) : (g_rectangele.x + g_rectangele.width)) << endl;
		cout << "y:" << ((g_rectangele.y - g_rectangele.width < g_rectangele.y + g_rectangele.width) ? (g_rectangele.y - g_rectangele.width) : (g_rectangele.y + g_rectangele.width)) << endl;
		cout << "width:" << static_cast<int>(abs(g_rectangele.width * 2)) << endl;
		//cout << "height:" << static_cast<int>(abs(g_rectangele.height * 2)) << endl;
		break;
	}
}
void DrawRectangle(cv::Mat& img, cv::Rect box){
	rectangle(img, Point(box.x-box.width,box.y-box.width),Point(box.x+box.width,box.y+box.width), Scalar(g_rng.uniform(0, 255), g_rng.uniform(0, 255), g_rng.uniform(0, 255)));
}
