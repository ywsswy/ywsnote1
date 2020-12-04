#include <opencv2/opencv.hpp>  
#include <opencv2/legacy/compat.hpp>  
using namespace std;  
#pragma comment(linker, "/subsystem:\"windows\" /entry:\"mainCRTStartup\"")  
void FillWhite(IplImage *pImage)
{
	cvRectangle(pImage, cvPoint(0, 0), cvPoint(pImage->width, pImage->height), CV_RGB(255, 255, 255), CV_FILLED);
}
// 创建灰度图像的直方图  
CvHistogram* CreateGrayImageHist(IplImage **ppImage)
{
	int nHistSize = 256;
	float fRange[] = { 0, 255 };  //灰度级的范围    
	float *pfRanges[] = { fRange };
	CvHistogram *pcvHistogram = cvCreateHist(1, &nHistSize, CV_HIST_ARRAY, pfRanges);
	cvCalcHist(ppImage, pcvHistogram);
	return pcvHistogram;
}
// 根据直方图创建直方图图像  
IplImage* CreateHisogramImage(int nImageWidth, int nScale, int nImageHeight, CvHistogram *pcvHistogram)
{
	IplImage *pHistImage = cvCreateImage(cvSize(nImageWidth * nScale, nImageHeight), IPL_DEPTH_8U, 1);
	FillWhite(pHistImage);
	//统计直方图中的最大直方块  
	float fMaxHistValue = 0;
	cvGetMinMaxHistValue(pcvHistogram, NULL, &fMaxHistValue, NULL, NULL);
	//分别将每个直方块的值绘制到图中  
	int i;
	for (i = 0; i < nImageWidth; i++)
	{
		float fHistValue = cvQueryHistValue_1D(pcvHistogram, i); //像素为i的直方块大小  
		int nRealHeight = cvRound((fHistValue / fMaxHistValue) * nImageHeight);  //要绘制的高度  
		cvRectangle(pHistImage,
			cvPoint(i * nScale, nImageHeight - 1),
			cvPoint((i + 1) * nScale - 1, nImageHeight - nRealHeight),
			cvScalar(i, 0, 0, 0),
			CV_FILLED
			);
	}
	return pHistImage;
}
int main(
#if 0 // import 2, argv[0]="MyOpenCV.exe",argv[1]="mr.jpg"
	int argc, char** argv
#endif
	){
	const char *pstrWindowsSrcTitle = "原图";
	const char *pstrWindowsGrayTitle = "灰度图";
	const char *pstrWindowsHistTitle = "直方图";
	// 从文件中加载原图  
	IplImage *pSrcImage = cvLoadImage("D:\\program files (x86)\\opencv\\projects\\MyOpenCV\\Debug\\323_199.png" , CV_LOAD_IMAGE_UNCHANGED);
	IplImage *pGrayImage = cvCreateImage(cvGetSize(pSrcImage), IPL_DEPTH_8U, 1);
	// 灰度图  
	cvCvtColor(pSrcImage, pGrayImage, CV_BGR2GRAY);
	// 灰度直方图  
	CvHistogram *pcvHistogram = CreateGrayImageHist(&pGrayImage);
	// 创建直方图图像  
	int nHistImageWidth = 255;
	int nHistImageHeight = 150;  //直方图图像高度  
	int nScale = 2;
	IplImage *pHistImage = CreateHisogramImage(nHistImageWidth, nScale, nHistImageHeight, pcvHistogram);
	// 显示  
	cvNamedWindow(pstrWindowsSrcTitle, CV_WINDOW_AUTOSIZE);
	cvNamedWindow(pstrWindowsGrayTitle, CV_WINDOW_AUTOSIZE);
	cvNamedWindow(pstrWindowsHistTitle, CV_WINDOW_AUTOSIZE);
	cvShowImage(pstrWindowsSrcTitle, pSrcImage);
	cvShowImage(pstrWindowsGrayTitle, pGrayImage);
	cvShowImage(pstrWindowsHistTitle, pHistImage);
	cvWaitKey(0);
	cvReleaseHist(&pcvHistogram);
	cvDestroyWindow(pstrWindowsSrcTitle);
	cvDestroyWindow(pstrWindowsGrayTitle);
	cvDestroyWindow(pstrWindowsHistTitle);
	cvReleaseImage(&pSrcImage);
	cvReleaseImage(&pGrayImage);
	cvReleaseImage(&pHistImage);
	return 0;
}
