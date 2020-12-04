int main()
{
	string st = R"(C:\Users\admin\Desktop\daea38dbb6fd5266874128b5a218972bd50736cd.jpg)";
	Mat srcImage = imread(st);
	imshow("【原图】", srcImage);
