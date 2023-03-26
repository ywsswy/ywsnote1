#pragma  comment(lib,"Msimg32.lib")  //添加使用TransparentBlt函数所需的库文件
SelectObject(YGlobalVars::hMdc,YGlobalVars::hBmCharacter1);
TransparentBlt(hdc,0,0,300,400,YGlobalVars::hMdc,0,0,200,400,RGB(235,97,0));//指定透明色
BOOL TransparentBlt(
  __in  HDC hdcDest,
  __in  int xoriginDest,
  __in  int yoriginDest,
  __in  int wDest,
  __in  int hDest,
  __in  HDC hdcSrc,
  __in  int xoriginSrc,
  __in  int yoriginSrc,
  __in  int wSrc,
  __in  int hSrc,
  __in  UINT crTransparent
);
//这个不同于BitBlt，这个就涉及到缩放了
hmdc... （x,y,width,height)
如果x+width 超过总宽度，就出错了，什么都不会画了

