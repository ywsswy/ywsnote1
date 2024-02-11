#include <opencv2/opencv.hpp>
using namespace cv;
namespace ycv {
	class Win {
	public:
		Win(string name,int type);
		void imshow(Mat &img);
		string name;
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
}
void on_Trackbar(int par1, void *pvoid)
{
	static void *p1 = (void*)(*(0+((int*)pvoid)));
	static void *p2 = (void*)(*(1 + ((int*)pvoid)));
	static void *p3 = (void*)(*(2 + ((int*)pvoid)));
	std::string *winname = (std::string*)p1;
	Mat *mgray = (Mat*)p2;
	Mat *mbin = (Mat*)p3;
	adaptiveThreshold(*mgray, *mbin, 255,
		CV_ADAPTIVE_THRESH_MEAN_C, CV_THRESH_BINARY, 
		par1>1 ? (par1%2==1 ? par1 : par1+1) : 3, 11);
	imshow(*winname, *mbin);
}
int main(int argc, char** argv)
{
	ycv::Win w1 = ycv::Win("窗口1", CV_WINDOW_NORMAL);
	Mat mgray = imread("1.jpg");
	Mat mbin;
	cvtColor(mgray, mgray, CV_BGR2GRAY);
	if (!mgray.data) { 
		std::cout << "read error\n" << std::endl;
		return -1;
	}
	int gs = 71;
	int *p[3];
	p[0] = (int*)&w1.name;
	p[1] = (int*)&mgray;
	p[2] = (int*)&mbin;
	createTrackbar("滑动条1", w1.name, &gs, 333, on_Trackbar,p);
	waitKey();
	return 0;
}
