////////////////////////////////////////
#ifndef CVGABOR_H
#define CGABOR_H
#include <iostream>
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>
const double PI = 3.14159265;
const int CV_GABOR_REAL = 1;
const int CV_GABOR_IMAG = 2;
const int CV_GABOR_MAG = 3;
const int CV_GABOR_PHASE = 4;
using namespace cv;
using namespace std;
class CvGabor{
public:
	CvGabor();
	~CvGabor();
	CvGabor(int iMu, int iNu);
	CvGabor(int iMu, int iNu, double dSigma);
	CvGabor(int iMu, int iNu, double dSigma, double dF);
	CvGabor(double dPhi, int iNu);
	CvGabor(double dPhi, int iNu, double dSigma);
	CvGabor(double dPhi, int iNu, double dSigma, double dF);
	void Init(int iMu, int iNu, double dSigma, double dF);
	void Init(double dPhi, int iNu, double dSigma, double dF);
	bool IsInit();
	bool IsKernelCreate();
	int mask_width();
	int get_mask_width();
	void get_image(int Type, Mat& image);
	void get_matrix(int Type, Mat& matrix);
	void conv_img(Mat& src, Mat& dst, int Type);
protected:
	double Sigma;
	double F;
	double Kmax;
	double K;
	double Phi;
	bool bInitialised;
	bool bKernel;
	int Width;
	Mat Imag;
	Mat Real;
private:
	void creat_kernel();
};
#endif
