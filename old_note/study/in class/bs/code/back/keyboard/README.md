//-----------------------------------【头文件包含部分】---------------------------------------
//	描述：包含程序所依赖的头文件
//------------------------------------------------------------------------------------------------
#include <windows.h>
#include "YLog.h"
#include <cstdlib>
//-----------------------------------【库文件包含部分】---------------------------------------
//	描述：包含程序所依赖的库文件
//------------------------------------------------------------------------------------------------
#pragma comment(lib,"winmm.lib")  //调用PlaySound函数所需库文件
#pragma  comment(lib,"Msimg32.lib")  //添加使用TransparentBlt函数所需的库文件
//-----------------------------------【全局变量声明部分】-------------------------------------
//	描述：全局变量的声明
//------------------------------------------------------------------------------------------------
class man{
public:
	man(){}
	void goUp(){
		this->y -= man::step;
	}
	void goDown(){
		this->y += man::step;
	}
	void goRight(){
		this->x += man::step;
	}
	void goLeft(){
		this->x -= man::step;
	}
	void setX(int x){this->x = x;}
	void setY(int y){this->y = y;}
	int getX(){return this->x;}
	int getY(){return this->y;}
	static const int step = 4;
private:
	int x;
	int y;
};
class YGlobalVars{
public:
	static LPCWSTR class_name;
	static LPCWSTR window_title;
	static const int window_width = 800;
	static const int window_height = 600;
	static const int window_x = 50;
	static const int window_y = 50;
	static const int man1_w = 40;
	static const int man1_h = 80;
	static YLog log1;
	static char bufc[100];
	template<typename T> static char* value2string16(T x){
		_itoa_s(x,YGlobalVars::bufc,16);
		return YGlobalVars::bufc;
	}
	template<typename T> static char* memery2string16(T x){
		//memcpy_s(YGlobalVars::bufc,sizeof(x)
	}
	static HPEN hPenSolidRed; //定义画笔句柄
	static HPEN hPenSolidBlack; //定义画笔句柄
	static HBRUSH hBrushCross; //定义画刷句柄
	static HBRUSH hBrushSolid; //定义画刷句柄
	static HDC hdc;
	static HDC hMdc;
	static HDC hBdc;
	static HBITMAP hBitmap;
	static HBITMAP hBmCharacter1;
	static LPCWSTR info1;
	static unsigned int lastkey;
	static DWORD tPre;
	static man man1;
};
YLog YGlobalVars::log1(YLog::INFO, "log1.txt",YLog::ADD);
LPCWSTR YGlobalVars::class_name = L"ForTheDreamOfGameDevelop";
LPCWSTR YGlobalVars::window_title = L"移动迷宫"; //窗口标题
char YGlobalVars::bufc[100] = {0};
HDC	YGlobalVars::hdc=NULL;
HDC	YGlobalVars::hMdc=NULL;
HDC	YGlobalVars::hBdc=NULL;
HPEN YGlobalVars::hPenSolidRed = CreatePen(PS_SOLID,1,RGB(255,0,0));
HPEN YGlobalVars::hPenSolidBlack = CreatePen(PS_SOLID,1,RGB(0,0,0));
HBRUSH YGlobalVars::hBrushCross = CreateHatchBrush(HS_DIAGCROSS,RGB(0,0,255));
HBRUSH YGlobalVars::hBrushSolid = CreateSolidBrush(RGB(0,255,0));
HBITMAP YGlobalVars::hBitmap = NULL;
HBITMAP YGlobalVars::hBmCharacter1 = NULL;
LPCWSTR YGlobalVars::info1 = L"副总监色哦佛阿伟";
unsigned int YGlobalVars::lastkey = 0;
DWORD YGlobalVars::tPre = 0;
man YGlobalVars::man1;
//-----------------------------------【全局函数声明部分】-------------------------------------
//	描述：全局函数声明，防止“未声明的标识”系列错误
//------------------------------------------------------------------------------------------------
LRESULT CALLBACK WndProc( HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam );
BOOL Game_Init(HWND hwnd, std::wstring &info);
void Game_Paint(HWND hwnd);
BOOL Game_CleanUp(HWND hwnd);
void Game_Paint(HWND hwnd){
	/*仅用户显式调用，不会系统调用*/
	//YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "Game_Paint start","");
	SelectObject(YGlobalVars::hBdc,YGlobalVars::hBitmap);
	BitBlt(YGlobalVars::hMdc,0,0,YGlobalVars::window_width,YGlobalVars::window_height,YGlobalVars::hBdc,0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小  
	
	SelectObject(YGlobalVars::hBdc,YGlobalVars::hBmCharacter1);
	TransparentBlt(YGlobalVars::hMdc,YGlobalVars::man1.getX(),YGlobalVars::man1.getY(),YGlobalVars::man1_w,YGlobalVars::man1_h,YGlobalVars::hBdc,0,0,YGlobalVars::man1_w,YGlobalVars::man1_h,RGB(0,0,0));//指定透明色
	BitBlt(YGlobalVars::hdc,0,0,YGlobalVars::window_width,YGlobalVars::window_height,YGlobalVars::hMdc,0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小  
	YGlobalVars::tPre = GetTickCount();
	return;
}
BOOL Game_Init(HWND hwnd, std::wstring &info){
	YGlobalVars::hdc = GetDC(hwnd);  //获取设备环境句柄
	YGlobalVars::hMdc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC
	YGlobalVars::hBdc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC
	HBITMAP bmp = CreateCompatibleBitmap(YGlobalVars::hdc,YGlobalVars::window_width,YGlobalVars::window_height); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMdc,bmp);//将空位图对象放到mdc中
	void * pLoadResource = NULL;
	pLoadResource = LoadImage(NULL,L"Naruto.bmp",IMAGE_BITMAP,YGlobalVars::window_width,YGlobalVars::window_height,LR_LOADFROMFILE);
	if(!pLoadResource){
		info = L"img map load faild.";
		return FALSE;
	}else{
		YGlobalVars::hBitmap = (HBITMAP)pLoadResource;
	}
	pLoadResource = LoadImage(NULL,L"man1.bmp",IMAGE_BITMAP,YGlobalVars::man1_w,YGlobalVars::man1_h,LR_LOADFROMFILE);
	if(!pLoadResource){
		info = L"man1 img load faild.";
		return FALSE;
	}else{
		YGlobalVars::hBmCharacter1 = (HBITMAP)pLoadResource;
	}
	YGlobalVars::man1.setX(150);
	YGlobalVars::man1.setY(250);
	return TRUE;
}
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance,LPSTR lpCmdLine, int nShowCmd)
{
	YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WinMain start","");
	WNDCLASSEX wndClass = { 0 };							//用WINDCLASSEX定义了一个窗口类
	wndClass.cbSize = sizeof( WNDCLASSEX ) ;			//设置结构体的字节数大小
	wndClass.style = CS_HREDRAW | CS_VREDRAW | CS_NOCLOSE;	//设置窗口的样式
	wndClass.lpfnWndProc = WndProc;					//设置指向窗口过程函数的指针
	wndClass.hInstance = hInstance;						//指定包含窗口过程的程序的实例句柄。
	wndClass.hIcon=(HICON)::LoadImage(NULL,L"icon.ico",IMAGE_ICON,0,0,LR_DEFAULTSIZE|LR_LOADFROMFILE);  //本地加载自定义ico图标
	wndClass.hCursor = LoadCursor( NULL, IDC_ARROW );    //指定窗口类的光标句柄。
	wndClass.hbrBackground=(HBRUSH)GetStockObject(WHITE_BRUSH);  //为hbrBackground成员指定一个白色画刷句柄	
	wndClass.lpszMenuName = NULL;//						//用一个以空终止的字符串，指定菜单资源的名字。
	wndClass.lpszClassName = YGlobalVars::class_name;		//用一个以空终止的字符串，指定。
	if( !RegisterClassEx( &wndClass ) )				//设计完窗口后，需要对窗口类进行注册，这样才能创建该类型的窗口
		return -1;		
	HWND hwnd = CreateWindow( YGlobalVars::class_name, YGlobalVars::window_title,				//喜闻乐见的创建窗口函数CreateWindow
		WS_OVERLAPPED|WS_CAPTION, YGlobalVars::window_x, YGlobalVars::window_y, YGlobalVars::window_width,
		YGlobalVars::window_height, NULL, NULL, hInstance, NULL );
	//Game_Init 放到了ShowWindow前面，不然，没init之前就会触发VM_PAINT了
	std::wstring info = L"";
	if (!Game_Init (hwnd, info)){
		MessageBox(hwnd, info.c_str(), L"资源初始化失败.", 0); //使用MessageBox函数，创建一个消息窗口
		Game_CleanUp(hwnd);
		return FALSE;
	}
	PlaySound(L"仙剑·重楼.wav", NULL, SND_FILENAME | SND_ASYNC | SND_LOOP); //循环播放背景音乐
	//MoveWindow(hwnd,250,80,WINDOW_WIDTH,WINDOW_HEIGHT,true);		//调整窗口显示时的位置，使窗口左上角位于（250,80）处
	ShowWindow( hwnd, nShowCmd );    //调用ShowWindow函数来显示窗口
	UpdateWindow(hwnd);						//对窗口进行更新，就像我们买了新房子要装修一样
	MSG msg = { 0 };
	while( msg.message != WM_QUIT ){
		/*
		if( PeekMessage( &msg, 0, 0, 0, PM_REMOVE ) )   //查看应用程序消息队列，有消息时将队列中的消息派发出去。
		{
			//TranslateMessage( &msg );		//将虚拟键消息转换为字符消息
			DispatchMessage( &msg );			//分发一个消息给窗口程序。
		} else{
			if(GetTickCount() - YGlobalVars::tPre >= 200){//0.1秒
				Game_Paint(hwnd);
			}
		}*/
		if(GetTickCount() - YGlobalVars::tPre >= 200){//0.1秒
			Game_Paint(hwnd);
		} else if( PeekMessage( &msg, 0, 0, 0, PM_REMOVE ) )   //查看应用程序消息队列，有消息时将队列中的消息派发出去。
		{
			//TranslateMessage( &msg );		//将虚拟键消息转换为字符消息
			DispatchMessage( &msg );			//分发一个消息给窗口程序。
		}
	}
	UnregisterClass(wndClass.lpszClassName, wndClass.hInstance);  //程序准备结束，注销窗口类
	YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WinMain end","");
	return 0;
}
LRESULT CALLBACK WndProc( HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam ){
	/*鼠标在区域内动都会触发处理程序，短按一个按键会触发三次*/
	switch( message ){
	case WM_DESTROY:					//若是窗口销毁消息
		//YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WndProc WMDESTROY","");
		Game_CleanUp(hwnd);			//调用自定义的资源清理函数Game_CleanUp（）进行退出前的资源清理
		PostQuitMessage( 0 );			//向系统表明有个线程有终止请求。用来响应WM_DESTROY消息
		break;									//跳出该switch语句
	case WM_KEYDOWN:					// 若是键盘按下消息
		YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WM_KEYDOWN",YGlobalVars::value2string16(wParam));
		//YGlobalVars::lastkey = wParam;
		switch(wParam){
		case VK_ESCAPE:
			DestroyWindow(hwnd);
			break;
		case VK_UP:
			YGlobalVars::man1.goUp();
			YGlobalVars::tPre = 0;
			break;
		case VK_DOWN:
			YGlobalVars::man1.goDown();
			YGlobalVars::tPre = 0;
			break;
		case VK_LEFT:
			YGlobalVars::man1.goLeft();
			YGlobalVars::tPre = 0;
			break;
		case VK_RIGHT:
			YGlobalVars::man1.goRight();
			YGlobalVars::tPre = 0;
			break;
		default:
			break;
		}
		break;									//跳出该switch语句
	case WM_LBUTTONDOWN:
		YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WM_LMOUSE_x",LOWORD(lParam));
		YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WM_LMOUSE_y",HIWORD(lParam));
		YGlobalVars::man1.setX(150);
		YGlobalVars::man1.setY(250);
		break;
	default:										//若上述case条件都不符合，则执行该default语句
		//YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WndProc default","");
		return DefWindowProc( hwnd, message, wParam, lParam );		//调用缺省的窗口过程
	}
	return 0;
}
BOOL Game_CleanUp( HWND hwnd ){
	YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "Game_CleanUp","");
	//释放掉所有的画笔和画刷句柄
	DeleteObject(YGlobalVars::hPenSolidRed);
	DeleteObject(YGlobalVars::hPenSolidBlack);
	DeleteObject(YGlobalVars::hBrushCross);
	DeleteObject(YGlobalVars::hBrushSolid);
	DeleteObject(YGlobalVars::hBitmap);
	DeleteObject(YGlobalVars::hBmCharacter1);
	DeleteDC(YGlobalVars::hBdc);
	DeleteDC(YGlobalVars::hMdc);
	ReleaseDC(hwnd,YGlobalVars::hdc);
	return TRUE;
}
