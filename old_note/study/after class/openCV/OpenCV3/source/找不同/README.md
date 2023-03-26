#include<iostream>
#include<string>
#include<vector>
#include<io.h>
#include <opencv2/opencv.hpp>
#include<sstream>
using namespace std;
using namespace cv;
int yStringToInt(string &s){
	int n = 0;
	stringstream ss;
	ss << s;
	ss >> n;
	return n;
}
void getFiles(string path, vector<string>& files, vector<string> &ownname)
{
	/*files存储文件的路径及名称(eg.   C:\Users\WUQP\Desktop\test_devided\data1.txt)
	ownname只存储文件的名称(eg.     data1.txt)*/
	//文件句柄  
	long   hFile = 0;
	//文件信息  
	struct _finddata_t fileinfo;
	string p;
	if ((hFile = _findfirst(p.assign(path).append("\\*").c_str(), &fileinfo)) != -1)
	{
		do
		{
			//如果是目录,迭代之  
			//如果不是,加入列表  
			if ((fileinfo.attrib &  _A_SUBDIR))
			{  /*
			   if(strcmp(fileinfo.name,".") != 0  &&  strcmp(fileinfo.name,"..") != 0)
			   getFiles( p.assign(path).append("\\").append(fileinfo.name), files, ownname ); */
			}
			else
			{
				files.push_back(p.assign(path).append("\\").append(fileinfo.name));
				ownname.push_back(fileinfo.name);
			}	
		} while (_findnext(hFile, &fileinfo) == 0);
		_findclose(hFile);
	}
}
int main(int argc,char ** argv){
	vector<string> path, file;
	getFiles(".", path, file);
	int len = file.size();
	string picname = "";
	int i = 0;
	for (i = 0; i < len; i++){
		if (file[i].find(".png", 0) != string::npos){
			picname = file[i];
			break;
		}
	}
	if (i == len){
		cout << "can not find" << endl;
		return 0;
	}
	cout << picname << endl;
	int type = 0;
	/*
	type :
	0 个人竞速 组队炸弹 个人炸弹
	1 每日一图  经典300关
	*/
	//cin >> type;
	string stype = argv[1];
	type = yStringToInt(stype);
	int X1[] = {135,145};
	int Y1[] = {65,113};
	int X2[] = {681,690};
	int Y2[] = {612,659};
	int Y3[] = {665,687};
	int x1 = X1[type];
	int y1 = Y1[type];
	int x2 = X2[type];
	int y2 = Y2[type];
	int y3 = Y3[type];
//
	namedWindow("windows1", CV_WINDOW_NORMAL);
	namedWindow("windows2", CV_WINDOW_NORMAL);
	namedWindow("windows3", CV_WINDOW_NORMAL);
	namedWindow("windows4", CV_WINDOW_NORMAL);
	Mat img = imread(picname);
	Mat img1 = img(Rect(x1, y1, x2 - x1, y2 - y1));
	Mat img2 = img(Rect(x1, y3, x2 - x1, y2 - y1));
	int h = img1.rows;
	int w = img1.cols;
	//692,658
	imshow("windows1", img1);
	imshow("windows2", img2);
	Mat view = img1.clone();
	Mat minus1(h, w, CV_8UC3);
	Mat minus2(h, w, CV_8UC3);
	Mat res(h, w, CV_8UC3);
	minus1 = img1 - img2;
	minus2 = img2 - img1;
	res = minus1 + minus2;
	imshow("windows3", res);
	for (int i = 0; i < h; i++){
		for (int j = 0; j < w; j++){
			int c1 = res.at<cv::Vec3b>(i, j)[0];
			int c2 = res.at<cv::Vec3b>(i, j)[1];
			int fin = (c1 + c2);
			if (fin > 8){
				view.at<Vec3b>(i, j)[0] /= 2;
				view.at<Vec3b>(i, j)[1] /= 2;
				view.at<Vec3b>(i, j)[2] = 255;
			}
			else{
				view.at<Vec3b>(i, j)[0] /= 6;
				view.at<Vec3b>(i, j)[1] /= 6;
				view.at<Vec3b>(i, j)[2] /= 6;
			}
		}
	}
	imshow("windows4", view);
	waitKey();
	remove(picname.c_str());
	return 0;
}

