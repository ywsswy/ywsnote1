////////////////////////////////////////////
#include <opencv2/opencv.hpp> 
using namespace std;
void y_move(IplImage *simg, IplImage *dimg, int yadd,int xadd){
	for (int y = 0; y < dimg->height; y++) {
		uchar* ptr = (uchar*)(dimg->imageData + y * dimg->widthStep);
		if (y - yadd >= 0 && y - yadd < simg->height){
			uchar* ptr2 = (uchar*)(simg->imageData + (y-yadd)*simg->widthStep);
			for (int x = 0; x < dimg->width; x++) {
				if (x - xadd >= 0 && x - xadd < simg->width){
					ptr[3 * x + 0] = ptr2[3 * (x - xadd) + 0];
					ptr[3 * x + 1] = ptr2[3 * (x - xadd) + 1];
					ptr[3 * x + 2] = ptr2[3 * (x - xadd) + 2];
				}
				else{
					ptr[3 * x + 0] = 255;
					ptr[3 * x + 1] = 255;
					ptr[3 * x + 2] = 255;
				}
			}
		}
		else{
			for (int x = 0; x < dimg->width; x++){
				ptr[3 * x + 0] = 255;
				ptr[3 * x + 1] = 255;
				ptr[3 * x + 2] = 255;
			}
		}
	}
}
void saturate_sv(IplImage* img) {
	for (int y = 0; y < img->height; y++) {
		uchar* ptr = (uchar*)(img->imageData + y * img->widthStep);
		//cvWaitKey(11);
		for (int x = 1; x < img->width; x++) {
			
				ptr[3 * x + 0] = 10;
				ptr[3 * x + 1] = 20;
				ptr[3 * x + 2] = 30;
		}
	}
}
int main(){
	const char *pstrImageName = "D:\\tt\\s1.jpg";
	const char *pstrSaveImageName = "D:\\tt\\m1.jpg";
	const char *pstrWindowsSrcTitle = "原图";
	const char *pstrWindowsDstTitle = "平移图";
	CvSize czSize;              //目标图像尺寸  
	IplImage *pSrcImage = cvLoadImage(pstrImageName, CV_LOAD_IMAGE_UNCHANGED);
	IplImage *pDstImage = NULL;
	czSize.width = pSrcImage->width;
	czSize.height = pSrcImage->height;
	cvNamedWindow(pstrWindowsSrcTitle, CV_WINDOW_AUTOSIZE);
	cvNamedWindow(pstrWindowsDstTitle, CV_WINDOW_AUTOSIZE);
	pDstImage = cvCreateImage(czSize, pSrcImage->depth, pSrcImage->nChannels);
	y_move(pSrcImage, pDstImage, 50, 100);
	cvShowImage(pstrWindowsSrcTitle, pSrcImage);
	cvShowImage(pstrWindowsDstTitle, pDstImage);
	cvWaitKey();
	cvSaveImage(pstrSaveImageName, pDstImage);
	cvDestroyWindow(pstrWindowsSrcTitle);
	cvReleaseImage(&pSrcImage);
	cvDestroyWindow(pstrWindowsDstTitle);
	cvReleaseImage(&pDstImage);
	return 0; 
}
