////////////////////////////
cv:Mat->IplImage
	cv::Mat histImg(histSize[0], histSize[0], CV_8UC3, cv::Scalar(255, 255, 255));
	//CvArr *histArr = &histImg;
	IplImage ipl_img(histImg);
【】
IplImage * ->cv:Mat 
IplImage* img;
cv:Mat mat(img);
【】
cv::Mat 转IplImage *
cv::Mat mat;
IplImage *img=&(IplImage)mat;
【】
 cv::Mat 转const cvArr*
Mat img;
const CvArr* s=(CvArr*)&img;
CvArr>CvMat>IplImage

