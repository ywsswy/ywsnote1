#ifndef HISTOGRAM_H_
#define HISTOGRAM_H_
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <iostream>
#include <vector>
class Histogram {
private:
	int histSize[1];
	float hrangee[2];
	const float* ranges[1];
	int channels[1];
protected:
	cv::Mat getHistogram(const cv::Mat&);
public:
	Histogram();
	cv::Mat getHistogramImage(const cv::Mat&, int channel = 0);
};
#endif /* HISTOGRAM_H_ */
Histogram::Histogram() {
	histSize[0] = 256;
	hrangee[0] = 0.0;
	hrangee[1] = 255.0;
	ranges[0] = hrangee;
	channels[0] = 0;
}
cv::Mat Histogram::getHistogram(const cv::Mat& image){
	cv::MatND hist;
	cv::calcHist(&image, 1, channels, cv::Mat(), hist, 1, histSize, ranges);
	return hist;
}
cv::Mat Histogram::getHistogramImage(const cv::Mat& image, int channel){
	std::vector<cv::Mat> planes;
	cv::split(image, planes);
	cv::Scalar color;
	if (planes.size() == 1){
		channel = 0;
		color = cv::Scalar(0, 0, 0);
	}
	else{
		color = cv::Scalar(channel == 0 ? 255 : 0, channel == 1 ? 255 : 0, channel == 2 ? 255 : 0);
	}
	cv::MatND hist = getHistogram(planes[channel]);
	double maxVal = 0;
	double minVal = 0;
	cv::minMaxLoc(hist, &minVal, &maxVal, 0, 0);
	cv::Mat histImg(histSize[0], histSize[0], CV_8UC3, cv::Scalar(255, 255, 255));
	int hpt = static_cast<int>(0.9*histSize[0]);
	for (int h = 0; h < histSize[0] - 1; h++){
		float binVal = hist.at<float>(h);
		float binVal2 = hist.at<float>(h + 1);
		int intensity = static_cast<int>(binVal*hpt / maxVal);
		int intensity2 = static_cast<int>(binVal2*hpt / maxVal);
		cv::line(histImg, cv::Point(h, histSize[0] - intensity),
			cv::Point(h, histSize[0] - intensity2), color);
	}
	return histImg;
}
int main(){
	cv::Mat image = cv::imread("D:\\program files (x86)\\opencv\\projects\\MyOpenCV\\Debug\\mr.jpg");
	//"E:/Image/Lena.jpg");
	Histogram h;
	cv::namedWindow("Red");
	cv::namedWindow("Blue");
	cv::namedWindow("Green");
	cv::namedWindow("Original");
	cv::imshow("Original", image);
	cv::imshow("Red", h.getHistogramImage(image, 2));
	cv::imshow("Green", h.getHistogramImage(image, 1));
	cv::imshow("Blue", h.getHistogramImage(image));
	cv::waitKey(0);
	return 0;
}
