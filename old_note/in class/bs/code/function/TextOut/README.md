	HFONT hFont=CreateFont(30,0,0,0,0,0,0,0,GB2312_CHARSET,0,0,0,0,L"微软雅黑");  //创建一种字体
	SelectObject(YGlobalVars::hdc,hFont);  //将字体选入设备环境中
	TextOut(YGlobalVars::hdc,30,150,YGlobalVars::info1,wcslen(YGlobalVars::info1));
	DeleteObject(hFont);//释放字体对象
