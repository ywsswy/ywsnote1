//-----------------------------------【头文件包含部分】---------------------------------------
//	描述：包含程序所依赖的头文件
//------------------------------------------------------------------------------------------------
#include <windows.h>
#include "YLog.h"
#include <cstdlib>
#include <vector>
#include <list>
//-----------------------------------【库文件包含部分】---------------------------------------
//	描述：包含程序所依赖的库文件
//------------------------------------------------------------------------------------------------
#pragma comment(lib,"winmm.lib")  //调用PlaySound函数所需库文件
#pragma comment(lib,"Msimg32.lib")  //添加使用TransparentBlt函数所需的库文件
//-----------------------------------【全局变量声明部分】-------------------------------------
//	描述：全局变量的声明
//------------------------------------------------------------------------------------------------
class BitmapInfo{
public:
	BitmapInfo(std::wstring path, int index, int x, int y, int w, int h):path_(path),bitmap_(0),index_(index),x_(x),y_(y),w_(w),h_(h){}
	std::wstring path_;
	HBITMAP bitmap_;
	int index_;
	int x_;
	int y_;
	int w_;
	int h_;
};
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
	static const int window_height = 400;
	static const int window_x = 450;//窗口初始化坐标
	static const int window_y = 50;//窗口初始化坐标
	static const int view_w_ = 400;//游戏区
	static const int view_h_ = 400;
	static const int view_x_ = 0;
	static const int view_y_ = 0;
	static const int code_w_ = 400;//代码区
	static const int code_h_ = 400;
	static const int code_x_ = 400;
	static const int code_y_ = 0;
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
	static HDC hdc;
	static HDC hMdc;
	static HDC hBdc;
	static HBITMAP hBitmap;
	static HBITMAP hBmCharacter1;
	static LPCWSTR info1;
	static unsigned int lastkey;
	static DWORD tPre;
	static man man1;
	static std::vector<BitmapInfo> bitmap_info_vector_;
	static std::list<BitmapInfo> bitmap_info_list_;
};
YLog YGlobalVars::log1(YLog::INFO, "data/loginfo.txt",YLog::ADD);
LPCWSTR YGlobalVars::class_name = L"class_name";
LPCWSTR YGlobalVars::window_title = L"移动迷宫"; //窗口标题
char YGlobalVars::bufc[100] = {0};
HDC	YGlobalVars::hdc=NULL;
HDC	YGlobalVars::hMdc=NULL;
HDC	YGlobalVars::hBdc=NULL;
HPEN YGlobalVars::hPenSolidRed = CreatePen(PS_SOLID,1,RGB(255,0,0));
HPEN YGlobalVars::hPenSolidBlack = CreatePen(PS_SOLID,1,RGB(0,0,0));
HBITMAP YGlobalVars::hBitmap = NULL;
HBITMAP YGlobalVars::hBmCharacter1 = NULL;
LPCWSTR YGlobalVars::info1 = L"副总监色哦佛阿伟";
unsigned int YGlobalVars::lastkey = 0;
DWORD YGlobalVars::tPre = 0;
man YGlobalVars::man1;
std::vector<BitmapInfo> YGlobalVars::bitmap_info_vector_;
std::list<BitmapInfo> YGlobalVars::bitmap_info_list_;
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
	YGlobalVars::bitmap_info_list_.clear();
	for(size_t i = 0;i<YGlobalVars::bitmap_info_vector_.size();i++){
		YGlobalVars::bitmap_info_list_.push_back(YGlobalVars::bitmap_info_vector_[i]);
	}
	//list.sort()
	for(std::list<BitmapInfo>::iterator it = YGlobalVars::bitmap_info_list_.begin();it != YGlobalVars::bitmap_info_list_.end();it++){
		SelectObject(YGlobalVars::hBdc,it->bitmap_);
		BitBlt(YGlobalVars::hMdc,it->x_,it->y_,it->w_,it->h_,YGlobalVars::hBdc,0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小  
	}
	BitBlt(YGlobalVars::hdc,0,0,YGlobalVars::window_width,YGlobalVars::window_height,YGlobalVars::hMdc,0,0,SRCCOPY);
	YGlobalVars::tPre = GetTickCount();
	return;
}
BOOL Game_Init(HWND hwnd, std::wstring &info){
	YGlobalVars::hdc = GetDC(hwnd);  //获取设备环境句柄
	YGlobalVars::hMdc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC（作为二级缓冲区）
	HBITMAP bmp = CreateCompatibleBitmap(YGlobalVars::hdc,YGlobalVars::window_width,YGlobalVars::window_height); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMdc,bmp);//将空位图对象放到mdc中
	DeleteObject(bmp);
	YGlobalVars::hBdc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC（作为顶级缓冲区）
	//0-4
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/code.bmp",0,YGlobalVars::code_x_,YGlobalVars::code_y_,YGlobalVars::code_w_,YGlobalVars::code_h_));
	for(size_t i = 0;i<YGlobalVars::bitmap_info_vector_.size();i++){
		void * pLoadResource = LoadImage(NULL,YGlobalVars::bitmap_info_vector_[i].path_.c_str(),IMAGE_BITMAP,YGlobalVars::bitmap_info_vector_[i].w_,YGlobalVars::bitmap_info_vector_[i].h_,LR_LOADFROMFILE);
		if(!pLoadResource){
			info = YGlobalVars::bitmap_info_vector_[i].path_;
			return FALSE;
		}else{
			YGlobalVars::bitmap_info_vector_[i].bitmap_ = (HBITMAP)pLoadResource;
		}
	}
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
	wndClass.hIcon=(HICON)::LoadImage(NULL,L"data/icon.ico",IMAGE_ICON,0,0,LR_DEFAULTSIZE|LR_LOADFROMFILE);  //本地加载自定义ico图标
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
	PlaySound(L"data/仙剑·重楼.wav", NULL, SND_FILENAME | SND_ASYNC | SND_LOOP); //循环播放背景音乐
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
		if(GetTickCount() - YGlobalVars::tPre >= 200){
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
	DeleteObject(YGlobalVars::hPenSolidRed);
	DeleteObject(YGlobalVars::hPenSolidBlack);
	for(size_t i = YGlobalVars::bitmap_info_vector_;i<YGlobalVars::bitmap_info_vector_.size();i++){
		DeleteObject(YGlobalVars::bitmap_info_vector_[i].bitmap_);
	}
	DeleteDC(YGlobalVars::hBdc);
	DeleteDC(YGlobalVars::hMdc);
	ReleaseDC(hwnd,YGlobalVars::hdc);
	return TRUE;
}
