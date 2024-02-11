安装python
pip：
python -m pip install -U pip 
numpy：
pip install numpy
安装OpenCV 
在官网自行下载，这里下载的是opencv2.4.10安装。 
http://sourceforge.net/projects/opencvlibrary/files/opencv-win/
1）复制cv2.pyd 
将”\opencv\build\python\2.7\x64”或”\opencv\build\python\2.7\x86”（根据python版本）文件夹中找到cv2.pyd”，复制到Python安装文件的”C:\Python27\Lib\site-packages”文件夹中。 
测试
执行解压目录下的sources\samples\python\drawing.py
import cv2
import numpy as np
img = cv2.imread("C:\lena.jpg")
cv2.imshow("lena",img)
cv2.waitKey(10000)
