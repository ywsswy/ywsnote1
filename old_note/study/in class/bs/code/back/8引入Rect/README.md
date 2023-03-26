#include <windows.h>
#include "YLog.h"
#include <iostream>
#include <string>
#include <cstdlib>
#include <vector>
#include <list>
#include <sstream>
#include <fstream>
#include <algorithm>
#pragma comment(lib,"winmm.lib")  //调用PlaySound函数所需库文件
#pragma comment(lib,"Msimg32.lib")  //添加使用TransparentBlt函数所需的库文件
class BitmapInfo{
public:
	BitmapInfo(std::wstring path, int index,RECT rect,bool flag):path_(path),bitmap_(0),rect_(rect),index_(index),trans_flag_(flag){}
	std::wstring path_;
	HBITMAP bitmap_;
	int index_;
	RECT rect_;
	bool trans_flag_;
};
bool operator<(const BitmapInfo &yc1,const BitmapInfo &yc2){
	return yc1.index_ < yc2.index_;
}
RECT re1;
class YGlobalVars{
public:
	static LPCWSTR class_name;
	static LPCWSTR window_title;
	static const int window_margin_right_ = 5;
	static const int window_margin_bottom_ = 30;
	static RECT window_rect_;
	static RECT view_rect_;
	static RECT code_rect_;
	static RECT btn_start_rect_;
	static RECT man_rect_;
	static const int box_w_ = 50;
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
	static HDC hMMdc[4];//四个方向的地图
	static DWORD tPre;
	static std::vector<BitmapInfo> bitmap_info_vector_;
	static std::list<BitmapInfo> bitmap_info_list_;
	static std::vector<std::vector<int> > cs_yws_map_;
	static int cs_yws_map_h_;//总行数
	static int cs_yws_map_w_;
	static int cs_yws_map_sr_;//起始坐标
	static int cs_yws_map_sc_;
	static int cs_yws_map_nr_;//现在坐标
	static int cs_yws_map_nc_;
	static int cs_yws_map_dir_;
	static int GetRectWidth(const RECT& rect){
		return rect.right - rect.left;
	}
	static int GetRectHeight(const RECT& rect){
		return rect.bottom - rect.top;
	}
	static void InitRectByXY(RECT& rect, int x1, int y1, int x2, int y2){
		rect.left = x1;
		rect.right = x2;
		rect.top = y1;
		rect.bottom = y2;
		return;
	}
	static void InitRectByWH(RECT& rect, int x, int y, int w, int h){
		rect.left = x;
		rect.right = x+w;
		rect.top = y;
		rect.bottom = y+h;
		return;
	}
	static YLog loginfo;
};
YLog YGlobalVars::loginfo(YLog::INFO,"data/loginfo.txt",YLog::ADD);
LPCWSTR YGlobalVars::class_name = L"class_name";
LPCWSTR YGlobalVars::window_title = L"移动迷宫"; //窗口标题
char YGlobalVars::bufc[100] = {0};
HDC	YGlobalVars::hdc=NULL;
HDC	YGlobalVars::hMdc=NULL;
HDC	YGlobalVars::hMMdc[4] = {0};
HPEN YGlobalVars::hPenSolidRed = CreatePen(PS_SOLID,1,RGB(255,0,0));
HPEN YGlobalVars::hPenSolidBlack = CreatePen(PS_SOLID,1,RGB(0,0,0));
DWORD YGlobalVars::tPre = 0;
std::vector<BitmapInfo> YGlobalVars::bitmap_info_vector_;
std::list<BitmapInfo> YGlobalVars::bitmap_info_list_;
std::vector<std::vector<int> > YGlobalVars::cs_yws_map_;
int YGlobalVars::cs_yws_map_h_ = 0;
int YGlobalVars::cs_yws_map_w_ = 0;
int YGlobalVars::cs_yws_map_sr_ = 0;
int YGlobalVars::cs_yws_map_sc_ = 0;
int YGlobalVars::cs_yws_map_nr_ = 0;
int YGlobalVars::cs_yws_map_nc_ = 0;
int YGlobalVars::cs_yws_map_dir_ = 0;
RECT YGlobalVars::view_rect_;
RECT YGlobalVars::code_rect_;
RECT YGlobalVars::btn_start_rect_;
RECT YGlobalVars::window_rect_;
RECT YGlobalVars::man_rect_;
//-----------------------------------【全局函数声明部分】-------------------------------------
//	描述：全局函数声明，防止“未声明的标识”系列错误
//------------------------------------------------------------------------------------------------
LRESULT CALLBACK WndProc( HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam );
BOOL Game_Init(HWND hwnd, std::wstring &info);
void Game_Paint(HWND hwnd);
BOOL Game_CleanUp(HWND hwnd);
void Game_Paint(HWND hwnd){
	HDC buf_dc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC（作为顶级缓冲区）
	//YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "Game_Paint start","");
	bool index_flag = false;//false-》没开始画人物物品
	YGlobalVars::bitmap_info_list_.clear();
	YGlobalVars::bitmap_info_list_.push_front(YGlobalVars::bitmap_info_vector_[0]);
	YGlobalVars::bitmap_info_list_.push_front(YGlobalVars::bitmap_info_vector_[17]);
	YGlobalVars::bitmap_info_list_.push_front(YGlobalVars::bitmap_info_vector_[18]);
	YGlobalVars::bitmap_info_list_.push_front(YGlobalVars::bitmap_info_vector_[19]);
	YGlobalVars::bitmap_info_list_.sort();
	for(std::list<BitmapInfo>::iterator it = YGlobalVars::bitmap_info_list_.begin();it != YGlobalVars::bitmap_info_list_.end();it++){
		if(it->index_>=10 && !index_flag){
			int nr_rot = 0;
			int nc_rot = 0;
			int x = 0;
			int y = 0;
			int w = 0;
			int h = 0;
			if(YGlobalVars::cs_yws_map_dir_==0){
				nr_rot = YGlobalVars::cs_yws_map_nr_;
				nc_rot = YGlobalVars::cs_yws_map_nc_;
				y = YGlobalVars::man_rect_.top-YGlobalVars::box_w_*nr_rot;
				x = YGlobalVars::man_rect_.left-YGlobalVars::box_w_*nc_rot;
				w = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_,YGlobalVars::view_rect_.right-x);
				h = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_,YGlobalVars::view_rect_.bottom-y); 
			}else if(YGlobalVars::cs_yws_map_dir_==1){
				nr_rot = YGlobalVars::cs_yws_map_w_-1-YGlobalVars::cs_yws_map_nc_;
				nc_rot = YGlobalVars::cs_yws_map_nr_;
				y = YGlobalVars::man_rect_.top-YGlobalVars::box_w_*nr_rot;
				x = YGlobalVars::man_rect_.left-YGlobalVars::box_w_*nc_rot;
				w = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_,YGlobalVars::view_rect_.right-x);
				h = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_,YGlobalVars::view_rect_.bottom-y);
			}else if(YGlobalVars::cs_yws_map_dir_==2){
				nr_rot = YGlobalVars::cs_yws_map_h_-1-YGlobalVars::cs_yws_map_nr_;
				nc_rot = YGlobalVars::cs_yws_map_w_-1-YGlobalVars::cs_yws_map_nc_;
				y = YGlobalVars::man_rect_.top-YGlobalVars::box_w_*nr_rot;
				x = YGlobalVars::man_rect_.left-YGlobalVars::box_w_*nc_rot;
				w = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_,YGlobalVars::view_rect_.right-x);
				h = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_,YGlobalVars::view_rect_.bottom-y);
			}else{
				nr_rot = YGlobalVars::cs_yws_map_nc_;
				nc_rot = YGlobalVars::cs_yws_map_h_-1-YGlobalVars::cs_yws_map_nr_;
				y = YGlobalVars::man_rect_.top-YGlobalVars::box_w_*nr_rot;
				x = YGlobalVars::man_rect_.left-YGlobalVars::box_w_*nc_rot;
				w = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_,YGlobalVars::view_rect_.right-x);
				h = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_,YGlobalVars::view_rect_.bottom-y);
			}
			BitBlt(YGlobalVars::hMdc,x,y,w,h,YGlobalVars::hMMdc[YGlobalVars::cs_yws_map_dir_],0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小 
			index_flag = true;
		}
		SelectObject(buf_dc,it->bitmap_);
		if(it->trans_flag_){
			TransparentBlt(YGlobalVars::hMdc,it->rect_.left,it->rect_.top,it->rect_.right-it->rect_.left,it->rect_.bottom-it->rect_.top,buf_dc,0,0,it->rect_.right-it->rect_.left,it->rect_.bottom-it->rect_.top,RGB(128,128,192));
		}else{
			BitBlt(YGlobalVars::hMdc,it->rect_.left,it->rect_.top,it->rect_.right-it->rect_.left,it->rect_.bottom-it->rect_.top,buf_dc,0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小
		}  
	}
	BitBlt(YGlobalVars::hdc,0,0,YGlobalVars::window_rect_.right-YGlobalVars::window_rect_.left,YGlobalVars::window_rect_.bottom-YGlobalVars::window_rect_.top,YGlobalVars::hMdc,0,0,SRCCOPY);
	DeleteDC(buf_dc);
	YGlobalVars::tPre = GetTickCount();
	return;
}
void MapStart(){
	YGlobalVars::cs_yws_map_nr_ = YGlobalVars::cs_yws_map_sr_;
	YGlobalVars::cs_yws_map_nc_ = YGlobalVars::cs_yws_map_sc_;
	YGlobalVars::cs_yws_map_dir_ = 0;
}
void MapInit(const int challenge){
	std::string filename = "data/map/";
	std::stringstream filess;
	filess << filename << challenge;
	filess >> filename;
	std::ifstream if1(filename);
	if (!if1){
		assert(0 && "map_data_file load failed");
	}
	std::streambuf* oldbuf = std::cin.rdbuf(if1.rdbuf());
	std::wstring line = L"";
	std::wstringstream ss;
	std::cin >> YGlobalVars::cs_yws_map_h_ >> YGlobalVars::cs_yws_map_w_;
	YGlobalVars::cs_yws_map_.clear();
	for(int i = 0;i<YGlobalVars::cs_yws_map_h_;i++){
		std::vector<int> bufrow;
		for(int j = 0;j<YGlobalVars::cs_yws_map_w_;j++){
			int bufint = 0;
			std::cin >> bufint;
			bufrow.push_back(bufint);
			if(bufint == 10){
				YGlobalVars::cs_yws_map_sr_ = i;
				YGlobalVars::cs_yws_map_sc_ = j;
			}
		}
		YGlobalVars::cs_yws_map_.push_back(bufrow);
	}
	for(int i = 0;i<4;i++){
		YGlobalVars::hMMdc[i] = CreateCompatibleDC(YGlobalVars::hdc);
	}
	HBITMAP bmp;
	bmp = CreateCompatibleBitmap(YGlobalVars::hdc,YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_,YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMMdc[0],bmp);//将空位图对象放到mdc中
	DeleteObject(bmp);
	bmp = CreateCompatibleBitmap(YGlobalVars::hdc,YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_,YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMMdc[2],bmp);//将空位图对象放到mdc中
	DeleteObject(bmp);
	bmp = CreateCompatibleBitmap(YGlobalVars::hdc,YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_,YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMMdc[1],bmp);//将空位图对象放到mdc中
	DeleteObject(bmp);
	bmp = CreateCompatibleBitmap(YGlobalVars::hdc,YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_,YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMMdc[3],bmp);//将空位图对象放到mdc中
	DeleteObject(bmp);
	HDC buf_dc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC（作为顶级缓冲区）
	for(int i = 0;i<YGlobalVars::cs_yws_map_h_;i++){
		for(int j = 0;j<YGlobalVars::cs_yws_map_w_;j++){
			if(YGlobalVars::cs_yws_map_[i][j]%10 == 1){
				SelectObject(buf_dc,YGlobalVars::bitmap_info_vector_[10].bitmap_);
			} else if(YGlobalVars::cs_yws_map_[i][j]%10 == 0){
				SelectObject(buf_dc,YGlobalVars::bitmap_info_vector_[5].bitmap_);
			} else if(YGlobalVars::cs_yws_map_[i][j]%10 == 3){
				SelectObject(buf_dc,YGlobalVars::bitmap_info_vector_[16].bitmap_);
			} else{
				assert(0&&"WTF");
			}
			BitBlt(YGlobalVars::hMMdc[0],j*YGlobalVars::box_w_,i*YGlobalVars::box_w_,YGlobalVars::box_w_,YGlobalVars::box_w_,buf_dc,0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小
			BitBlt(YGlobalVars::hMMdc[1],i*YGlobalVars::box_w_,(YGlobalVars::cs_yws_map_w_-1-j)*YGlobalVars::box_w_,YGlobalVars::box_w_,YGlobalVars::box_w_,buf_dc,0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小
			BitBlt(YGlobalVars::hMMdc[2],(YGlobalVars::cs_yws_map_w_-1-j)*YGlobalVars::box_w_,(YGlobalVars::cs_yws_map_h_-1-i)*YGlobalVars::box_w_,YGlobalVars::box_w_,YGlobalVars::box_w_,buf_dc,0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小
			BitBlt(YGlobalVars::hMMdc[3],(YGlobalVars::cs_yws_map_h_-1-i)*YGlobalVars::box_w_,j*YGlobalVars::box_w_,YGlobalVars::box_w_,YGlobalVars::box_w_,buf_dc,0,0,SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小  
		}
	}
	DeleteDC(buf_dc);
	return;
}
BOOL Game_Init(HWND hwnd, std::wstring &info){
	YGlobalVars::hdc = GetDC(hwnd);  //获取设备环境句柄
	YGlobalVars::hMdc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC（作为二级缓冲区）
	HBITMAP bmp = CreateCompatibleBitmap(YGlobalVars::hdc,YGlobalVars::window_rect_.right-YGlobalVars::window_rect_.left,YGlobalVars::window_rect_.bottom-YGlobalVars::window_rect_.top); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMdc,bmp);//将空位图对象放到mdc中
	DeleteObject(bmp);
	//0-4
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/code.bmp",0,YGlobalVars::code_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner30.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner01.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner12.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner23.bmp",0,YGlobalVars::man_rect_,false));
	//5-9
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner_white.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner00.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner11.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner22.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner33.bmp",0,YGlobalVars::man_rect_,false));
	//10-14
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner_black.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner_e0.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner_e1.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner_e2.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner_e3.bmp",0,YGlobalVars::man_rect_,false));
	//15-19
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner0123.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/corner_trap.bmp",0,YGlobalVars::man_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/view.bmp",0,YGlobalVars::view_rect_,false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/man.bmp",10,YGlobalVars::man_rect_,true));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo(L"data/btn_start.bmp",1,YGlobalVars::btn_start_rect_,true));
	//20-24
	for(size_t i = 0;i<YGlobalVars::bitmap_info_vector_.size();i++){
		void * pLoadResource = LoadImage(NULL,YGlobalVars::bitmap_info_vector_[i].path_.c_str(),IMAGE_BITMAP,
			YGlobalVars::GetRectWidth(YGlobalVars::bitmap_info_vector_[i].rect_),
			YGlobalVars::GetRectHeight(YGlobalVars::bitmap_info_vector_[i].rect_),LR_LOADFROMFILE);
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
	YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WinMain start","");
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
	YGlobalVars::InitRectByWH(YGlobalVars::view_rect_,0,0,600,600);
	YGlobalVars::InitRectByWH(YGlobalVars::code_rect_,YGlobalVars::view_rect_.right,0,600,600);
	YGlobalVars::InitRectByWH(YGlobalVars::btn_start_rect_,YGlobalVars::code_rect_.left,YGlobalVars::code_rect_.bottom,150,70);
	YGlobalVars::InitRectByWH(YGlobalVars::window_rect_,50,50,
		YGlobalVars::GetRectWidth(YGlobalVars::view_rect_)+YGlobalVars::GetRectWidth(YGlobalVars::code_rect_)+YGlobalVars::window_margin_right_,
		YGlobalVars::GetRectHeight(YGlobalVars::code_rect_)+YGlobalVars::GetRectHeight(YGlobalVars::btn_start_rect_)+YGlobalVars::window_margin_bottom_);
	YGlobalVars::InitRectByWH(YGlobalVars::man_rect_,YGlobalVars::GetRectWidth(YGlobalVars::view_rect_)/2-YGlobalVars::box_w_/2,
		YGlobalVars::GetRectHeight(YGlobalVars::view_rect_)*2/3-YGlobalVars::box_w_/2,YGlobalVars::box_w_,YGlobalVars::box_w_);
	HWND hwnd = CreateWindow( YGlobalVars::class_name, YGlobalVars::window_title,				//喜闻乐见的创建窗口函数CreateWindow
		WS_OVERLAPPED|WS_CAPTION, YGlobalVars::window_rect_.left, YGlobalVars::window_rect_.top,
		YGlobalVars::GetRectWidth(YGlobalVars::window_rect_),YGlobalVars::GetRectHeight(YGlobalVars::window_rect_), NULL, NULL, hInstance, NULL );
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
	MapInit(1);
	MapStart();
	while( msg.message != WM_QUIT ){
		/*
		if( PeekMessage( &msg, 0, 0, 0, PM_REMOVE ) )   //查看应用程序消息队列，有消息时将队列中的消息派发出去。
		{
			//TranslateMessage( &msg );		//将虚拟键消息转换为字符消息
			DispatchMessage( &msg );			//分发一个消息给窗口程序。
		} else{
			if(GetTickCount() - YGlobalVars::tPre >= 1000){//0.1秒
				YGlobalVars::cs_yws_map_dir_++;
				YGlobalVars::cs_yws_map_dir_ %= 4;
				Game_Paint(hwnd);
			}
		}
		*/
		if(GetTickCount() - YGlobalVars::tPre >= 500){
			Game_Paint(hwnd);
		} else if( PeekMessage( &msg, 0, 0, 0, PM_REMOVE ) )   //查看应用程序消息队列，有消息时将队列中的消息派发出去。
		{
			//TranslateMessage( &msg );		//将虚拟键消息转换为字符消息
			DispatchMessage( &msg );			//分发一个消息给窗口程序。
		}
	}
	UnregisterClass(wndClass.lpszClassName, wndClass.hInstance);  //程序准备结束，注销窗口类
	YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WinMain end","");
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
		YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WM_KEYDOWN",YGlobalVars::value2string16(wParam));
		switch(wParam){
		case VK_ESCAPE:
			DestroyWindow(hwnd);
			break;
		case VK_UP:
			if(YGlobalVars::cs_yws_map_dir_ == 0){
				YGlobalVars::cs_yws_map_nr_--;
			} else if(YGlobalVars::cs_yws_map_dir_ == 1){
				YGlobalVars::cs_yws_map_nc_++;
			} else if(YGlobalVars::cs_yws_map_dir_ == 2){
				YGlobalVars::cs_yws_map_nr_++;
			} else {
				YGlobalVars::cs_yws_map_nc_--;
			}
			YGlobalVars::tPre = 0;
			break;
		case VK_LEFT:
			YGlobalVars::cs_yws_map_dir_+=3;
			YGlobalVars::cs_yws_map_dir_%=4;
			YGlobalVars::tPre = 0;
			break;
		case VK_RIGHT:
			YGlobalVars::cs_yws_map_dir_+=1;
			YGlobalVars::cs_yws_map_dir_%=4;
			YGlobalVars::tPre = 0;
			break;
		default:
			break;
		}
		break;									//跳出该switch语句
	case WM_LBUTTONDOWN:
		YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WM_LMOUSE_x",LOWORD(lParam));
		YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WM_LMOUSE_y",HIWORD(lParam));
		break;
	default:										//若上述case条件都不符合，则执行该default语句
		//YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WndProc default","");
		return DefWindowProc( hwnd, message, wParam, lParam );		//调用缺省的窗口过程
	}
	return 0;
}
BOOL Game_CleanUp( HWND hwnd ){
	YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "Game_CleanUp","");
	DeleteObject(YGlobalVars::hPenSolidRed);
	DeleteObject(YGlobalVars::hPenSolidBlack);
	for(size_t i = 0;i<YGlobalVars::bitmap_info_vector_.size();i++){
		DeleteObject(YGlobalVars::bitmap_info_vector_[i].bitmap_);
	}
	DeleteDC(YGlobalVars::hMdc);
	for(int i = 0;i<4;i++){
		DeleteDC(YGlobalVars::hMMdc[i]);
	}
	ReleaseDC(hwnd,YGlobalVars::hdc);
	return TRUE;
}
