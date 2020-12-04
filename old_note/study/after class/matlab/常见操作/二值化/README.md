///////////////////////////////////
close all;
img1=imread('D:\tt\192.168.1.64_01_2017010610415349.jpg');
img1=imresize(img1,0.60301703163017);
moving = imcrop(img1,[360 210 300 300]);
figure(),imshow(moving);
thresh = rgb2gray(moving);     %自动确定二值化阈值
I2 = im2bw(thresh);
figure(),imshow(I2),colormap(flipud(gray));

