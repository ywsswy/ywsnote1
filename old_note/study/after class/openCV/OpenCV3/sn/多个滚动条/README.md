#include <opencv2/opencv.hpp>
using namespace cv;
namespace ycv {
#include<iostream>
	class Win {
	public:
		Win(string name,int type);
		void imshow(Mat &img);
		std::string getName();
	private:
		std::string name;
	};
}
namespace ycv {
	Win::Win(string name, int type) {
		this->name = name;
		namedWindow(this->name, type);
	}
	void Win::imshow(Mat &img) {
		cv::imshow(this->name,img);
	}
	std::string Win::getName() {
		return this->name;
	}
}
void on_Trackbar3(int par, void *pvoid)//固定二值化
{
	static void *p0 = (void*)(*(0 + ((int*)pvoid)));
	static void *p1 = (void*)(*(1 + ((int*)pvoid)));
	static void *p2 = (void*)(*(2 + ((int*)pvoid)));
	static void *p3 = (void*)(*(3 + ((int*)pvoid)));
	static void *p4 = (void*)(*(4 + ((int*)pvoid)));
	ycv::Win *win = (ycv::Win*)p0;
	Mat *mgray = (Mat*)p1;
	Mat *mbin = (Mat*)p2;
	int *bs = (int*)p3;
	int *value = (int*)p4;
	*value = par;//diff
	threshold(*mgray, *mbin, *value, 255, CV_THRESH_BINARY);
	imshow(win->getName(), *mbin);
}
void on_Trackbar1(int par, void *pvoid)
{
	static void *p0 = (void*)(*(0 + ((int*)pvoid)));
	static void *p1 = (void*)(*(1 + ((int*)pvoid)));
	static void *p2 = (void*)(*(2 + ((int*)pvoid)));
	static void *p3 = (void*)(*(3 + ((int*)pvoid)));
	static void *p4 = (void*)(*(4 + ((int*)pvoid)));
	ycv::Win *win = (ycv::Win*)p0;
	Mat *mgray = (Mat*)p1;
	Mat *mbin = (Mat*)p2;
	int *bs = (int*)p3;
	int *value = (int*)p4;
	*bs = par;//diff
	adaptiveThreshold(*mgray, *mbin, 255,
		CV_ADAPTIVE_THRESH_MEAN_C, CV_THRESH_BINARY,
		*bs = (*bs > 1 ? ((*bs) % 2 == 1 ? *bs : *bs + 1) : 3),
		*value);
	win->imshow(*mbin);
}
void on_Trackbar2(int par, void *pvoid)
{
	static void *p0 = (void*)(*(0 + ((int*)pvoid)));
	static void *p1 = (void*)(*(1 + ((int*)pvoid)));
	static void *p2 = (void*)(*(2 + ((int*)pvoid)));
	static void *p3 = (void*)(*(3 + ((int*)pvoid)));
	static void *p4 = (void*)(*(4 + ((int*)pvoid)));
	ycv::Win *win = (ycv::Win*)p0;
	Mat *mgray = (Mat*)p1;
	Mat *mbin = (Mat*)p2;
	int *bs = (int*)p3;
	int *value = (int*)p4;
	*value = par;//diff
	adaptiveThreshold(*mgray, *mbin, 255,
		CV_ADAPTIVE_THRESH_MEAN_C, CV_THRESH_BINARY,
		*bs = (*bs > 1 ? ((*bs) % 2 == 1 ? *bs : *bs + 1) : 3),
		*value);
	win->imshow(*mbin);
}
int main(int argc, char** argv)
{
	ycv::Win w1 = ycv::Win("窗口1", CV_WINDOW_NORMAL);
	Mat mgray = imread("1.jpg");
	Mat mbin;
	if (!mgray.data) { 
		std::cout << "read error\n" << std::endl;
		return -1;
	}
	cvtColor(mgray, mgray, CV_BGR2GRAY);
	/*/
	cvtColor(mgray, mgray, CV_BGR2HSV);
	//提取通道
	Mat h;
	Mat s;
	Mat v;
	std::vector<cv::Mat> sbgr(mgray.channels());
	split(mgray, sbgr);//分离通道
	h = sbgr[0];
	s = sbgr[1];
	v = sbgr[2];
	
	*/
	//cvtColor(r, mgray, CV_BGR2GRAY);
	int bs = 11;
	int value = 11;
	int *p[5];
	p[0] = (int*)&w1;
	p[1] = (int*)&mgray;
	p[2] = (int*)&mbin;
	p[3] = (int*)&bs;
	p[4] = (int*)&value;
	createTrackbar("滑动条1", w1.getName(), &bs, 1111, on_Trackbar1, p);
	createTrackbar("滑动条2", w1.getName(), &value, 55, on_Trackbar2, p);
	//createTrackbar("滚动条3", w1.getName(), &value, 255, on_Trackbar3, p);
	waitKey();
	return 0;
}
