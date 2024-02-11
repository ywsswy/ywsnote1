#include <opencv2/opencv.hpp>
namespace ycv {
#include<iostream>
	class Win {
	public:
		Win(std::string name, int type);
		void imshow(cv::Mat &img);
		std::string getName();
	private:
		std::string name;
	};
}
namespace ycv {
	Win::Win(std::string name, int type) {
		this->name = name;
		cv::namedWindow(this->name, type);
	}
	void Win::imshow(cv::Mat &img) {
		cv::imshow(this->name, img);
	}
	std::string Win::getName() {
		return this->name;
	}
}
void on_Trackbar1(int par, void *pvoid)
{
	static void *p0 = (void*)(*(0 + ((int*)pvoid)));
	static void *p1 = (void*)(*(1 + ((int*)pvoid)));
	static void *p2 = (void*)(*(2 + ((int*)pvoid)));
	static void *p3 = (void*)(*(3 + ((int*)pvoid)));
	static void *p4 = (void*)(*(4 + ((int*)pvoid)));
	ycv::Win *win = (ycv::Win*)p0;
	cv::Mat *mgray = (cv::Mat*)p1;
	cv::Mat *mbin = (cv::Mat*)p2;
	int *bs = (int*)p3;
	int *value = (int*)p4;
	*bs = par;//diff
	adaptiveThreshold(*mgray, *mbin, 255,
		cv::ADAPTIVE_THRESH_MEAN_C, cv::THRESH_BINARY,
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
	cv::Mat *mgray = (cv::Mat*)p1;
	cv::Mat *mbin = (cv::Mat*)p2;
	int *bs = (int*)p3;
	int *value = (int*)p4;
	*value = par;//diff
	adaptiveThreshold(*mgray, *mbin, 255,
		cv::ADAPTIVE_THRESH_MEAN_C, cv::THRESH_BINARY,
		*bs = (*bs > 1 ? ((*bs) % 2 == 1 ? *bs : *bs + 1) : 3),
		*value);
	win->imshow(*mbin);
}
void on_Trackbar3(int par, void *pvoid)
{
	static void *p0 = (void*)(*(0 + ((int*)pvoid)));
	static void *p1 = (void*)(*(1 + ((int*)pvoid)));
	static void *p2 = (void*)(*(2 + ((int*)pvoid)));
	static void *p3 = (void*)(*(3 + ((int*)pvoid)));
	static void *p4 = (void*)(*(4 + ((int*)pvoid)));
	ycv::Win *win = (ycv::Win*)p0;
	cv::Mat *mgray = (cv::Mat*)p1;
	cv::Mat *mbin = (cv::Mat*)p2;
	int *bs = (int*)p3;
	int *value = (int*)p4;
	*value = par;//dif
	threshold(*mgray, *mbin, *value, 255, cv::THRESH_BINARY);
	win->imshow(*mbin);
}
int main(int argc, char** argv)
{
	//读取图片、创建窗口
	ycv::Win w1 = ycv::Win("窗口1", CV_WINDOW_NORMAL);
	cv::Mat mgray = cv::imread("out1.jpg");
	cv::Mat mbin;
	if (!mgray.data) {
		std::cout << "read error\n" << std::endl;
		return -1;
	}
#if 1 //转换为HSV空间
	cvtColor(mgray, mgray, CV_BGR2HSV);
#endif
#if 1 //分离通道
	std::vector<cv::Mat> sbgr(mgray.channels());
	split(mgray, sbgr);
#endif
#if 0 //合并通道
	std::vector<cv::Mat> mbgr(mgray.channels());
	mbgr[0] = sbgr[2];//or sbgr.at(2);
	mbgr[1] = sbgr[1];
	mbgr[2] = sbgr[0];
	merge(mbgr, mgray);
#endif
#if 0 //灰度化
	cvtColor(mgray, mgray, CV_BGR2GRAY);
#endif
	mgray = sbgr[0];
	int bs = 11;
	int value = 11;
	int *p[5];
	p[0] = (int*)&w1;
	p[1] = (int*)&mgray;
	p[2] = (int*)&mbin;
	p[3] = (int*)&bs;
	p[4] = (int*)&value;
#if 0 //OSTU法
	cv::threshold(mgray, mbin, 0, 255, cv::THRESH_BINARY | cv::THRESH_OTSU);//thresh is not setted manually
	w1.imshow(mbin);
#elif 0 //自适应二值化
	cv::createTrackbar("滑动条1", w1.getName(), &bs, 1111, on_Trackbar1,p);//多个滚动条，需要指明用户参数
	cv::createTrackbar("滑动条2", w1.getName(), &value, 255, on_Trackbar2);
	on_Trackbar2(bs, p);//显示调用一次，避免未进入之前画面无法显示
#else //固定二值化
	cv::createTrackbar("滚动条3", w1.getName(), &value, 255, on_Trackbar3);//单个滚动条，可以不指明用户参数，通过下面的显示调用来指明
	on_Trackbar3(value, p);
#endif
	cv::waitKey();
	return 0;
}
