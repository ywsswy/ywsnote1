	Rectangle(g_hdc,120,70,150,170);
//Rectangle(__in HDC hdc, __in int left, __in int top, __in int right, __in int bottom);
	//画出一个封闭的矩形，内部填充是画刷，外部线条是画笔
如果上下文没有用SelectObject选择其他画笔，和画刷
这个默认就是用宽为1的黑色画笔画矩形，内部填充空白？
