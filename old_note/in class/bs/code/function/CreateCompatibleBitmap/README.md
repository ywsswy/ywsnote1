YGlobalVars::hdc = GetDC(hwnd);  //获取设备环境句柄
YGlobalVars::hMdc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC
YGlobalVars::hBdc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC
HBITMAP bmp = CreateCompatibleBitmap(YGlobalVars::hdc,YGlobalVars::window_width,YGlobalVars::window_height); //建一个和窗口兼容的空的位图对象
SelectObject(YGlobalVars::hMdc,bmp);//将空位图对象放到mdc中
//为了防止闪烁现象，采取三缓冲方式
hdc由GetDC而来，对应最直观的屏幕显示（最外层缓冲），大小即为客户区大小
有了大小，就可以接收/被贴图
CreateCompatibleDC 而来的DC是没有大小的
1）可以通过SeleteObject指定LoadImage创建的句柄，这样此DC就可以把句柄对象贴图出去，但此DC并没有大小，只能贴图，不能被贴图。（此DC仅可以作为最内层缓冲）
2）可以通过SeleteObject指定CreateCompatibleBitmap创建的句柄，可以给此DC指定其大小，这样此DC就可以被贴图了。（此DC就可以作为中间层缓冲）
