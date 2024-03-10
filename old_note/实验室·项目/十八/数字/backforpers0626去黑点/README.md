#include<opencv2/opencv.hpp>
#include<iostream>
#include<string>
#define YTEST2
using namespace std;
using namespace cv;
#ifdef YTEST2
int main() {
	int argc = 11;
	char *argv[11] = {
		".exe",
		/*argv[1] = */"1",
		/*argv[2] = */"685",
		/*argv[3] = */"404",
		/*argv[4] = */"700",
		/*argv[5] = */"640",
		/*argv[6] = */"1228",
		/*argv[7] = */"624",
		/*argv[8] = */"1210",
		/*argv[9] = */"389",
	//	/*argv[10] = */R"(F:\tmp\2017-05-11数字（电子）\2017-05-11\0.269.jpg)"
		R"(F:\back\receive\qq\TestImage\bin\测试图片\20170511数字num_0618测试图\0.269-3.jpg)"
	};
#else
int main(int argc, char*argv[])
{
#endif
	if (argc != 11) {
		std::cout << "wrong param" << argc << std::endl;
		return 1;//param wrong
	}
	else {
		Mat img = imread(argv[10]);
		int img_height = img.rows;
		int img_width = img.cols;
		FileStorage yfs1("ymain.yaml", FileStorage::READ);
		string ytype = "yt";
		ytype.append(argv[1]);
		string yx1 = ytype + "_x1";
		string yy1 = ytype + "_y1";
		vector<Point2f> corners(4);//目标图
		vector<Point2f> srcTri(4);//源图
		yfs1[ytype + "_x1"] >> corners[0].x;
		yfs1[ytype + "_y1"] >> corners[0].y;
		yfs1[ytype + "_x1"] >> corners[1].x;
		yfs1[ytype + "_y2"] >> corners[1].y;
		yfs1[ytype + "_x2"] >> corners[3].x;
		yfs1[ytype + "_y1"] >> corners[3].y;
		yfs1[ytype + "_x2"] >> corners[2].x;
		yfs1[ytype + "_y2"] >> corners[2].y;
		srcTri[0].x = atof(argv[2]);
		srcTri[0].y = atof(argv[3]);
		srcTri[1].x = atof(argv[4]);
		srcTri[1].y = atof(argv[5]);
		srcTri[2].x = atof(argv[6]);
		srcTri[2].y = atof(argv[7]);
		srcTri[3].x = atof(argv[8]);
		srcTri[3].y = atof(argv[9]);
		Mat TArray = getPerspectiveTransform(srcTri, corners);
		Mat MArray = TArray.inv();
		FileStorage yfs2("ycal.yaml", FileStorage::WRITE);
		//p12 row:1 col2
		yfs2 << "p11" << MArray.at<double>(0, 0);
		yfs2 << "p12" << MArray.at<double>(0, 1);
		yfs2 << "p13" << MArray.at<double>(0, 2);
		yfs2 << "p21" << MArray.at<double>(1, 0);
		yfs2 << "p22" << MArray.at<double>(1, 1);
		yfs2 << "p23" << MArray.at<double>(1, 2);
		yfs2 << "p31" << MArray.at<double>(2, 0);
		yfs2 << "p32" << MArray.at<double>(2, 1);
		yfs2 << "p33" << MArray.at<double>(2, 2);
		yfs2.release();
		//现阶段会出现黑色空缺
		vector<Point2f> points(img_width*img_height), points_trans(img_width*img_height);
		for (int i = 0; i < img_height; i++) {
			for (int j = 0; j < img_width; j++) {
				points[i*img_width + j] = Point2f(j, i);
			}
		}
		perspectiveTransform(points, points_trans, MArray);
		Mat img_trans = Mat::zeros(img_height, img_width, CV_8UC3);
		int count = 0;
		for (int i = 0; i < img_height; i++) {
			uchar* t = img_trans.ptr<uchar>(i);
			for (int j = 0; j < img_width; j++) {
				int y = points_trans[count].y;
				int x = points_trans[count].x;
				count++;
				if (y < 0 || x < 0 || y + 1 >= img_height || x + 1 >= img_width)
					continue;
				uchar* p = img.ptr<uchar>(y);
				t[j * 3] = p[x * 3];
				t[j * 3 + 1] = p[x * 3 + 1];
				t[j * 3 + 2] = p[x * 3 + 2];
				/*
				t[x * 3] = p[j * 3];
				t[x * 3 + 1] = p[j * 3 + 1];
				t[x * 3 + 2] = p[j * 3 + 2];
				*/
			}
		}
		imwrite("ycal.jpg", img_trans);
		imshow("ycal.jpg", img_trans);
		waitKey();
	}
	return 0;
}
//x1,y1 x4,y4
//x2,y2 x3,y3
//corner[0] 3
//1			2
/*
paramtype = atoi(argv[1]);
paramx1 = atof(argv[2]);
paramy1 = atof(argv[3]);
paramx2 = atof(argv[4]);
paramy2 = atof(argv[5]);
paramx3 = atof(argv[6]);
paramy3 = atof(argv[7]);
paramx4 = atof(argv[8]);
paramy4 = atof(argv[9]);
std::cout << "type:" << paramtype << std::endl;
std::cout << "x1:" << paramx1 << std::endl;
std::cout << "y1:" << paramy1 << std::endl;
std::cout << paramx2 << std::endl;
std::cout << paramy2 << std::endl;
std::cout << paramx3 << std::endl;
std::cout << paramy3 << std::endl;
std::cout << paramx4 << std::endl;
std::cout << paramy4 << std::endl;
std::cout << "path:" << argv[10] << std::endl;
*/
