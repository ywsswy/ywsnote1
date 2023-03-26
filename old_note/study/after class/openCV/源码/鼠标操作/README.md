#include <opencv2/opencv.hpp>  
using namespace cv;
using namespace std;
bool flag = false;
Point center;
int radius = 3;
vector<Point> allPoints;
void onMouse(int event, int x, int y, int flags, void* param)
{
	if (event == CV_EVENT_LBUTTONDOWN)
	{
		flag = true;
		center = Point(x, y);
// 		cout << center.x << endl;
// 		cout << center.y<< endl;
		allPoints.push_back(center);
	}
	else if (event == CV_EVENT_LBUTTONUP)
	{
		flag = false;
	}
}
int main()
{
	Mat img(480, 640, CV_8UC3, Scalar(255, 255, 255));
	namedWindow("IMG");
	setMouseCallback("IMG", onMouse);
	while (1)
	{
		if (flag){
			circle(img, center, radius, Scalar(0, 0, 255), -1);
			imshow("IMG", img);
		}
		if (waitKey(10) == 27) break;
	}
	for (unsigned int i = 0; i < allPoints.size(); ++i)
		cout << allPoints[i] << endl;
	return 0;
}
