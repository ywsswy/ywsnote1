http://blog.csdn.net/poem_qianmo/article/details/19809337#comments
VC10(2010)
VC11(2012)
VC12(2013)
VC14(2015)
win
64/32
解压
环境变量bin，dll
视图-》属性管理器（一劳永逸）（debug-》micro（
C++目录
include包含目录.h
lib库目录bin
链接器-》输入-》附加依赖项
opencv_ts300d.lib
opencv_world300d.lib
#include<opencv2/opencv.hpp>
using namespace cv;
int main(){
	Mat img(200,300, CV_8UC3, Scalar(255, 0,0));
	imshow("hh", img);
	waitKey();
	return 0;
}
opencv_objdetect249.lib
opencv_ts249.lib
opencv_video249.lib
opencv_nonfree249.lib
opencv_ocl249.lib
opencv_photo249.lib
opencv_stitching249.lib
opencv_superres249.lib
opencv_videostab249.lib
opencv_calib3d249.lib
opencv_contrib249.lib
opencv_core249.lib
opencv_features2d249.lib
opencv_flann249.lib
opencv_gpu249.lib
opencv_highgui249.lib
opencv_imgproc249.lib
opencv_legacy249.lib
opencv_ml249.lib
