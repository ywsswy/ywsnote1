#include <opencv2/opencv.hpp>
#include <ctime>
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
	cv::Mat *cutImage = (cv::Mat*)p1;
	int *pb = (int*)p2;
	int *pg = (int*)p3;
	int *pr = (int*)p4;
	*pb = par;
	cv::Mat copyImage = *cutImage;
	std::vector<cv::Mat>channels(copyImage.channels());
	cv::split(copyImage, channels);
	double para[3] = { (double)(*pb) / 10, (double)(*pg) / 10, (double)(*pr) / 10 };
	for (int k = 0; k < 3; k++) {
		for (int i = 0; i < channels.at(k).rows; i++) {
			for (int j = 0; j < channels.at(k).cols; j++) {
				int buf = channels.at(k).at<unsigned char>(i, j) * para[k];
				if (buf > 255) {
					channels.at(k).at<unsigned char>(i, j) = 255;
				}
				else {
					channels.at(k).at<unsigned char>(i, j) = buf;
				}
			}
		}
	}
	cv::Mat res;
	merge(channels, res);
	win->imshow(res);
}
void on_Trackbar2(int par, void *pvoid)
{
	static void *p0 = (void*)(*(0 + ((int*)pvoid)));
	static void *p1 = (void*)(*(1 + ((int*)pvoid)));
	static void *p2 = (void*)(*(2 + ((int*)pvoid)));
	static void *p3 = (void*)(*(3 + ((int*)pvoid)));
	static void *p4 = (void*)(*(4 + ((int*)pvoid)));
	ycv::Win *win = (ycv::Win*)p0;
	cv::Mat *cutImage = (cv::Mat*)p1;
	int *pb = (int*)p2;
	int *pg = (int*)p3;
	int *pr = (int*)p4;
	*pg = par;
	cv::Mat copyImage = *cutImage;
	std::vector<cv::Mat>channels(copyImage.channels());
	cv::split(copyImage, channels);
	double para[3] = { (double)(*pb) / 10, (double)(*pg) / 10, (double)(*pr) / 10 };
	for (int k = 0; k < 3; k++) {
		for (int i = 0; i < channels.at(k).rows; i++) {
			for (int j = 0; j < channels.at(k).cols; j++) {
				int buf = channels.at(k).at<unsigned char>(i, j) * para[k];
				if (buf > 255) {
					channels.at(k).at<unsigned char>(i, j) = 255;
				}
				else {
					channels.at(k).at<unsigned char>(i, j) = buf;
				}
			}
		}
	}
	cv::Mat res;
	merge(channels, res);
	win->imshow(res);
}
void on_Trackbar3(int par, void *pvoid)
{
	static void *p0 = (void*)(*(0 + ((int*)pvoid)));
	static void *p1 = (void*)(*(1 + ((int*)pvoid)));
	static void *p2 = (void*)(*(2 + ((int*)pvoid)));
	static void *p3 = (void*)(*(3 + ((int*)pvoid)));
	static void *p4 = (void*)(*(4 + ((int*)pvoid)));
	ycv::Win *win = (ycv::Win*)p0;
	cv::Mat *cutImage = (cv::Mat*)p1;
	int *pb = (int*)p2;
	int *pg = (int*)p3;
	int *pr = (int*)p4;
	*pr = par;
	cv::Mat copyImage = *cutImage;
	std::vector<cv::Mat>channels(copyImage.channels());
	cv::split(copyImage, channels);
	double para[3] = { (double)(*pb) / 10, (double)(*pg) / 10, (double)(*pr) / 10 };
	for (int k = 0; k < 3; k++) {
		for (int i = 0; i < channels.at(k).rows; i++) {
			for (int j = 0; j < channels.at(k).cols; j++) {
				int buf = channels.at(k).at<unsigned char>(i, j) * para[k];
				if (buf > 255) {
					channels.at(k).at<unsigned char>(i, j) = 255;
				}
				else {
					channels.at(k).at<unsigned char>(i, j) = buf;
				}
			}
		}
	}
	cv::Mat res;
	merge(channels, res);
	win->imshow(res);
}
int main(int argc, char** argv)
{
	//读取图片、创建窗口
	ycv::Win w1 = ycv::Win("窗口1", CV_WINDOW_NORMAL);
	cv::Mat srcImage = cv::imread("E:\\file\\pic\\projects\\secret.jpg");
	if (!srcImage.data) {
		std::cout << "read error\n" << std::endl;
		return -1;
	}
	cv::Mat cutImage = srcImage(cv::Rect(20, 266, 538 - 20, 657 - 266));
	int *p[5];
	int temp = 10 * 10;
	int blue = temp;
	int green = temp;
	int red = temp;
	p[0] = (int*)&w1;
	p[1] = (int*)&cutImage;
	p[2] = (int*)&blue;
	p[3] = (int*)&green;
	p[4] = (int*)&red;
	//多个滚动条，需要指明用户参数
	cv::createTrackbar("blue", w1.getName(), &blue, temp * 2, on_Trackbar1, p);
	cv::createTrackbar("green", w1.getName(), &green, temp * 2, on_Trackbar2, p);
	cv::createTrackbar("red", w1.getName(), &red, temp * 2, on_Trackbar3);
	on_Trackbar3(red, p);//显示调用一次，避免未进入之前画面无法显示
	cv::waitKey();
	return 0;
}
