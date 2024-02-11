////////////////////////
View->Other Windows->Image Watch


如果不用这个插件，也可以在编译opencv的时候，勾选对qt以及opencgl的支持。以这样的方式编译得出的opencv，cv::imshow()函数渲染的窗口是opengl绘制的，支持放大图像至像素级，可查看像素点的R、G、B值大小
https://blog.mangoeffect.net/opencv/opencv-debug-by-imshow-instead-of-imagewatch