/////////////////////////////////////////
#include <opencv2/opencv.hpp>  
using namespace std;
void saturate_sv(IplImage* img) {
	for (int y = 0; y < img->height; y++) {
		uchar* ptr = (uchar*)(img->imageData + y * img->widthStep);
		//cvWaitKey(11);
		for (int x = 0; x < img->width / 4; x++) {
			ptr[3 * x + 0] = 0;
			ptr[3 * x + 1] = 0;
			ptr[3 * x + 2] = 255;
			//	for (int iii = 0; ptr[3 * x + 2] < 255&&iii<50; iii++){
			//		ptr[3 * x + 2]++;
			//	}
		}
	}
}
int main(){
	const char *pstrImageName = "D:\\tt\\s1.jpg";
	const char *pstrSaveImageName = "D:\\tt\\t1.jpg";
	const char *pstrWindowsSrcTitle = "原图";
	const char *pstrWindowsDstTitle = "改变图";
	IplImage *pSrcImage = cvLoadImage(pstrImageName, CV_LOAD_IMAGE_UNCHANGED);
	IplImage *pDstImage = NULL;
	cvNamedWindow(pstrWindowsSrcTitle, CV_WINDOW_AUTOSIZE);
	saturate_sv(pSrcImage);
	cvShowImage(pstrWindowsSrcTitle, pSrcImage);
	cvWaitKey();
	cvSaveImage(pstrSaveImageName, pSrcImage);
	cvDestroyWindow(pstrWindowsSrcTitle);
	cvReleaseImage(&pSrcImage);
	return 0;
}
	

