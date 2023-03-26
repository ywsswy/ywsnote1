#include <opencv2/opencv.hpp>
#include <iostream>
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
int main(){
	g_rectangele = Rect(-1, -1, 0, 0);
	Mat src = imread("E:\\JSH-3C.png");
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
	switch (event){//相应不同的鼠标事件  
	case EVENT_MOUSEMOVE:{
							 if (g_bDrawingBox){
								 g_rectangele.width = x - g_rectangele.x;
								 g_rectangele.height = y - g_rectangele.y;
							 }
	}
		break;
	case EVENT_LBUTTONDOWN:{
							   g_bDrawingBox = true;
							   g_rectangele = Rect(x, y, 0, 0);
	}
		break;
	case EVENT_LBUTTONUP:{
							 g_bDrawingBox = false;
							 if (g_rectangele.width < 0){
								 g_rectangele.x += g_rectangele.width;
								 g_rectangele.width *= -1;
							 }
							 if (g_rectangele.height < 0){
								 g_rectangele.y += g_rectangele.height;
								 g_rectangele.height *= -1;
							 }
							 DrawRectangle(image, g_rectangele);
	}
		break;
	}
}
void DrawRectangle(cv::Mat& img, cv::Rect box){//绘制矩形  
	printf("%d %d\n", box.width, box.height);
	rectangle(img, box.tl(), box.br(), Scalar(g_rng.uniform(0, 255), g_rng.uniform(0, 255), g_rng.uniform(0, 255)));
}
