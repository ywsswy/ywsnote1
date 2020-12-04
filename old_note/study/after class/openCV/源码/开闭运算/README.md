#include "cv.h"
#include "highgui.h"
using namespace cv;
int main(int argc, char* argv[])
{
        Mat src = imread("D:\\program files (x86)\\opencv\\projects\\MyOpenCV\\Debug\\rr.png", 0);
        Mat dst;
        
        //输入图像
        //输出图像
        //单元大小，这里是5*5的8位单元
        //腐蚀位置，为负值取核中心
        //腐蚀次数两次
        erode(src,dst,Mat(5,5,CV_8U),Point(-1,-1),2);
        imwrite("erode.jpg",dst);
        //输入图像
        //输出图像
        //单元大小，这里是5*5的8位单元
        //膨胀位置，为负值取核中心
        //膨胀次数两次
        dilate(src,dst,Mat(5,5,CV_8U),Point(-1,-1),2);
        imwrite("dilate.jpg",dst);
        //输入图像
        //输出图像
        //定义操作：MORPH_OPEN为开操作，MORPH_CLOSE为闭操作
        //单元大小，这里是3*3的8位单元
        //开闭操作位置
        //开闭操作次数
        morphologyEx(src,dst,MORPH_OPEN,Mat(3,3,CV_8U),Point(-1,-1),1);
        imwrite("open.jpg",dst);
        morphologyEx(src,dst,MORPH_CLOSE,Mat(3,3,CV_8U),Point(-1,-1),1);
        imwrite("close.jpg",dst);
        imshow("dst",dst);
        waitKey();
        return 0;
}
