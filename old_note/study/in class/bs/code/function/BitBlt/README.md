SelectObject(YGlobalVars::hMdc,YGlobalVars::hBitmap);    //将位图对象选入到g_mdc内存DC中
BitBlt(YGlobalVars::hdc,0,0,YGlobalVars::window_width,YGlobalVars::window_height/2,YGlobalVars::hMdc,0,0,SRCCOPY);    
BOOL BitBlt(
  __in  HDC hdcDest,
  __in  int nXDest,
  __in  int nYDest,
  __in  int nWidth,
  __in  int nHeight,
  __in  HDC hdcSrc,
  __in  int nXSrc,
  __in  int nYSrc,
  __in  DWORD dwRop
);
//前四个参数表示hdc的ROI区域，超过这个区域的mdc部分不会画到hdc上（空白）？
所以BitBlt不涉及到图片的缩放，仅仅可能类似剪裁
