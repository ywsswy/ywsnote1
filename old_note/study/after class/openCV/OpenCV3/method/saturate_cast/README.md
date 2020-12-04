///////////////////////////////
saturate_cast<uchar>()
溢出保护
原理类似：
if(data<0)
data = 0;
else if(data > 255)
data = 255;

