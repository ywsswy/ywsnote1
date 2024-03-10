#include <windows.h>
#include "ylog.h"
#include <iostream>
#include <string>
#include <vector>
#include <list>
#include <map>
#include <sstream>
#include <fstream>
#include <algorithm>
#include <thread>
#include "curl/curl.h"
#pragma comment(lib,"winmm.lib")  //调用PlaySound函数所需库文件
#pragma comment(lib,"Msimg32.lib")  //添加使用TransparentBlt函数所需的库文件
#pragma comment(lib,"libcurl.lib")  
std::string wstring2string(const std::wstring &ws){
	std::string res;
	for (size_t i = 0; i<ws.size(); i++){
		res.push_back(static_cast<char>(ws[i]));
	}
	return res;
}
std::wstring string2wstring(const std::string &ws){
	std::wstring res;
	for (size_t i = 0; i<ws.size(); i++){
		res.push_back(static_cast<wchar_t>(static_cast<unsigned char>(ws[i])));
	}
	return res;
}
class BitmapInfo{
public:
	BitmapInfo(std::string path, double index, RECT rect, bool flag) :path_(path), bitmap_(0), rect_(rect), index_(index), trans_flag_(flag){}
	std::string path_;
	HBITMAP bitmap_;
	double index_;//same as id
	RECT rect_;
	bool trans_flag_;
};
bool operator<(const BitmapInfo &yc1, const BitmapInfo &yc2){
	return yc1.index_ < yc2.index_;
}
RECT re1;
class YGlobalVars{
public:
	static const int window_margin_right_ = 5;
	static const int window_margin_bottom_ = 30;
	static RECT window_rect_;
	static RECT view_rect_;
	static RECT view_compiling_rect_;
	static RECT view_result_rect_;
	static RECT code_rect_;
	static RECT btn_start_rect_;
	static RECT btn_content_rect_;
	static RECT btn_exit_rect_;
	static RECT btn_stop_rect_;
	static RECT man_rect_;
	static const int box_w_ = 50;
	template<typename T> static char* value2string16(T x){
		static char bufc[100] = "";
		_itoa_s(x, bufc, 16);
		return bufc;
	}
	static HPEN hPenSolidRed; //定义画笔句柄
	static HPEN hPenSolidBlack; //定义画笔句柄
	static HDC hdc;
	static HDC hMdc;
	static HDC hMMdc[4];//四个方向的地图
	static DWORD tPre;
	static DWORD pre_t_cmd_;
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
	static void InitRectByRect(RECT& rect, const RECT& src){
		rect.left = src.left;
		rect.right = src.right;
		rect.top = src.top;
		rect.bottom = src.bottom;
		return;
	}
	static void InitRectByWH(RECT& rect, int x, int y, int w, int h){
		rect.left = x;
		rect.right = x + w;
		rect.top = y;
		rect.bottom = y + h;
		return;
	}
	static double JudgeClick(const int x, const int y){
		for (std::list<BitmapInfo>::reverse_iterator it = YGlobalVars::bitmap_info_list_.rbegin(); it != YGlobalVars::bitmap_info_list_.rend(); it++){
			if (x >= it->rect_.left && x <= it->rect_.right && y >= it->rect_.top && y <= it->rect_.bottom){
				return it->index_;
			}
		}
		return -1;
	}
	static YLog loginfo;
	static int flag_start_;
	static int flag_compiling_;
	static int flag_result_;
	static std::string compile_info_;
	static std::map<std::string, int> cmd_id_;
	static std::vector<std::map<int, std::string> > cmd_list_;
	static int cmd_index_;// 0:一个命令都没执行
};
std::map<std::string, int> YGlobalVars::cmd_id_;
std::vector<std::map<int, std::string> > YGlobalVars::cmd_list_;
int YGlobalVars::cmd_index_ = 0;// 0:一个命令都没执行
YLog YGlobalVars::loginfo(YLog::INFO, "data/loginfo.txt", YLog::ADD);
HDC	YGlobalVars::hdc = NULL;
HDC	YGlobalVars::hMdc = NULL;
HDC	YGlobalVars::hMMdc[4] = { 0 };
HPEN YGlobalVars::hPenSolidRed = CreatePen(PS_SOLID, 1, RGB(255, 0, 0));
HPEN YGlobalVars::hPenSolidBlack = CreatePen(PS_SOLID, 1, RGB(0, 0, 0));
DWORD YGlobalVars::tPre = 0;
DWORD YGlobalVars::pre_t_cmd_ = 0;
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
RECT YGlobalVars::view_compiling_rect_;
RECT YGlobalVars::view_result_rect_;
RECT YGlobalVars::code_rect_;
RECT YGlobalVars::btn_start_rect_;
RECT YGlobalVars::btn_content_rect_;
RECT YGlobalVars::btn_exit_rect_;
RECT YGlobalVars::btn_stop_rect_;
RECT YGlobalVars::window_rect_;
RECT YGlobalVars::man_rect_;
int YGlobalVars::flag_start_ = 0;//0: unstarted = display start button, 1: started = display stop button
int YGlobalVars::flag_compiling_ = 0;//0:undisplay the_view_compling
int YGlobalVars::flag_result_ = 0;//1:编译有报错信息
std::string YGlobalVars::compile_info_ = "";
LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam);
BOOL Game_Init(HWND hwnd, std::wstring &info);
BOOL Game_CleanUp(HWND hwnd);
static size_t DownloadCallback(char *buffer, size_t sz, size_t nmemb, void *writer){
	std::string* psResponse = (std::string*) writer;
	psResponse->append(buffer, sz * nmemb);
	return sz * nmemb;
}
CURLcode HTTPGet(const std::string &url, std::string &res){
	res = "";
	CURL *curl = curl_easy_init();
	curl_easy_setopt(curl, CURLOPT_URL, url.c_str());
	curl_easy_setopt(curl, CURLOPT_NOSIGNAL, 1L);
	curl_easy_setopt(curl, CURLOPT_TIMEOUT, 2L);
	//curl_easy_setopt(curl, CURLOPT_VERBOSE, 1); //显式详细信息
	curl_easy_setopt(curl, CURLOPT_SSL_VERIFYPEER, 0L);
	curl_easy_setopt(curl, CURLOPT_SSL_VERIFYHOST, 0L);
	curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, DownloadCallback);
	curl_easy_setopt(curl, CURLOPT_WRITEDATA, &res);
	CURLcode res_code = curl_easy_perform(curl);
	curl_easy_cleanup(curl);
	return res_code;
}
std::string UrlEncode(const std::vector<std::string> &vec_cs){
	std::string raw_cs = "";
	for (std::vector<std::string>::const_iterator it = vec_cs.begin(); it != vec_cs.end(); it++){
		raw_cs += *it;
		raw_cs += "\r\n";
	}
	std::string result;
	for (unsigned int i = 0; i< static_cast<unsigned int>(raw_cs.length()); i++){
		unsigned char ch = unsigned char(raw_cs[i]);
		if (ch == ' '){
			result += '+';
		}
		else if (ch >= 'A' && ch <= 'Z'){
			result += ch;
		}
		else if (ch >= 'a' && ch <= 'z'){
			result += ch;
		}
		else if (ch >= '0' && ch <= '9'){
			result += ch;
		}
		else if (ch == '-' || ch == '-' || ch == '.' || ch == '!' || ch == '~' || ch == '*' || ch == '\'' || ch == '(' || ch == ')'){
			result += ch;
		}
		else{
			result += "%";
			result += "0123456789abcdef"[unsigned int(ch) / 16];
			result += "0123456789abcdef"[unsigned int(ch) % 16];
		}
	}
	return result;
}
void CsRawRead(const std::string &filename, std::vector<std::string> &vec_cs){
	std::string line = "";
	std::ifstream if1(filename.c_str());
	while (getline(if1, line)){
		vec_cs.push_back(line);
	}
	return;
}
void CsInit(const int challenge, std::vector<std::string> &vec_cs){
	vec_cs.clear();
	std::string line = "";
	std::stringstream ss;
	line = "g_yws_map_r = ";
	ss.clear();
	ss << line << YGlobalVars::cs_yws_map_h_;
	getline(ss, line);
	vec_cs.push_back(line);
	line = "g_yws_map_c = ";
	ss.clear();
	ss << line << YGlobalVars::cs_yws_map_w_;
	getline(ss, line);
	vec_cs.push_back(line);
	for (int i = 0; i<YGlobalVars::cs_yws_map_h_; i++){
		line = "g_yws_map[";
		ss.clear();
		ss << line << i << "] = [";
		for (int j = 0; j<YGlobalVars::cs_yws_map_w_; j++){
			int bufint = YGlobalVars::cs_yws_map_[i][j];
			ss << bufint;
			if (j != YGlobalVars::cs_yws_map_w_ - 1){
				ss << ',';
			}
			else{
				ss << ']';
			}
		}
		getline(ss, line);
		vec_cs.push_back(line);
	}
	line = "g_yws_map_sr = ";
	ss.clear();
	ss << line << YGlobalVars::cs_yws_map_sr_;
	getline(ss, line);
	vec_cs.push_back(line);
	line = "g_yws_map_sc = ";
	ss.clear();
	ss << line << YGlobalVars::cs_yws_map_sc_;
	getline(ss, line);
	vec_cs.push_back(line);
	return;
}
void Game_Paint(HWND hwnd){
	HDC buf_dc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC（作为顶级缓冲区）
	//YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "Game_Paint start","");
	bool index_flag = false;//false-》没开始画人物物品
	std::list<BitmapInfo> bitmap_info_list;
	bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[0]);
	bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[17]);
	bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[18]);
	if (YGlobalVars::flag_start_ == 0){
		bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[19]);
	}
	else{
		bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[22]);
	}
	bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[20]);
	bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[21]);
	if (YGlobalVars::flag_compiling_){
		bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[23]);
	}
	if (YGlobalVars::flag_result_ == 1){
		bitmap_info_list.push_front(YGlobalVars::bitmap_info_vector_[24]);
	}
	bitmap_info_list.sort();
	YGlobalVars::bitmap_info_list_ = bitmap_info_list;
	//if (YGlobalVars::flag_compiling_ == 0 && YGlobalVars::flag_result_ == 0 && YGlobalVars::cmd_index_ < YGlobalVars::cmd_list_.size()){
	if (YGlobalVars::flag_start_ == 1 && YGlobalVars::cmd_index_ < YGlobalVars::cmd_list_.size()){
		if (GetTickCount() - YGlobalVars::pre_t_cmd_ > 1500){
			YGlobalVars::pre_t_cmd_ = GetTickCount();
			int cmd_id = YGlobalVars::cmd_list_[YGlobalVars::cmd_index_].begin()->first;
			if (cmd_id == 1){
				if (YGlobalVars::cs_yws_map_dir_ == 0){
					YGlobalVars::cs_yws_map_nr_--;
				}
				else if (YGlobalVars::cs_yws_map_dir_ == 1){
					YGlobalVars::cs_yws_map_nc_++;
				}
				else if (YGlobalVars::cs_yws_map_dir_ == 2){
					YGlobalVars::cs_yws_map_nr_++;
				}
				else {
					YGlobalVars::cs_yws_map_nc_--;
				}
			}
			else if (cmd_id == 3){//turn left
				YGlobalVars::cs_yws_map_dir_ += 3;
				YGlobalVars::cs_yws_map_dir_ %= 4;
			}
			else if (cmd_id == 4){
				YGlobalVars::cs_yws_map_dir_ += 1;
				YGlobalVars::cs_yws_map_dir_ %= 4;
			}
			YGlobalVars::cmd_index_++;
			if (YGlobalVars::cmd_index_ >= YGlobalVars::cmd_list_.size()){
				YGlobalVars::flag_start_ = 0;
			}
		}
	}
	else{
		YGlobalVars::pre_t_cmd_ = GetTickCount();
	}
	for (std::list<BitmapInfo>::iterator it = bitmap_info_list.begin(); it != bitmap_info_list.end(); it++){
		if (it->index_ >= 10 && !index_flag){
			int nr_rot = 0;
			int nc_rot = 0;
			int x = 0;
			int y = 0;
			int w = 0;
			int h = 0;
			if (YGlobalVars::cs_yws_map_dir_ == 0){
				nr_rot = YGlobalVars::cs_yws_map_nr_;
				nc_rot = YGlobalVars::cs_yws_map_nc_;
				y = YGlobalVars::man_rect_.top - YGlobalVars::box_w_*nr_rot;
				x = YGlobalVars::man_rect_.left - YGlobalVars::box_w_*nc_rot;
				w = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_, YGlobalVars::view_rect_.right - x);
				h = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_, YGlobalVars::view_rect_.bottom - y);
			}
			else if (YGlobalVars::cs_yws_map_dir_ == 1){
				nr_rot = YGlobalVars::cs_yws_map_w_ - 1 - YGlobalVars::cs_yws_map_nc_;
				nc_rot = YGlobalVars::cs_yws_map_nr_;
				y = YGlobalVars::man_rect_.top - YGlobalVars::box_w_*nr_rot;
				x = YGlobalVars::man_rect_.left - YGlobalVars::box_w_*nc_rot;
				w = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_, YGlobalVars::view_rect_.right - x);
				h = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_, YGlobalVars::view_rect_.bottom - y);
			}
			else if (YGlobalVars::cs_yws_map_dir_ == 2){
				nr_rot = YGlobalVars::cs_yws_map_h_ - 1 - YGlobalVars::cs_yws_map_nr_;
				nc_rot = YGlobalVars::cs_yws_map_w_ - 1 - YGlobalVars::cs_yws_map_nc_;
				y = YGlobalVars::man_rect_.top - YGlobalVars::box_w_*nr_rot;
				x = YGlobalVars::man_rect_.left - YGlobalVars::box_w_*nc_rot;
				w = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_, YGlobalVars::view_rect_.right - x);
				h = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_, YGlobalVars::view_rect_.bottom - y);
			}
			else{
				nr_rot = YGlobalVars::cs_yws_map_nc_;
				nc_rot = YGlobalVars::cs_yws_map_h_ - 1 - YGlobalVars::cs_yws_map_nr_;
				y = YGlobalVars::man_rect_.top - YGlobalVars::box_w_*nr_rot;
				x = YGlobalVars::man_rect_.left - YGlobalVars::box_w_*nc_rot;
				w = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_, YGlobalVars::view_rect_.right - x);
				h = min(YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_, YGlobalVars::view_rect_.bottom - y);
			}
			BitBlt(YGlobalVars::hMdc, x, y, w, h, YGlobalVars::hMMdc[YGlobalVars::cs_yws_map_dir_], 0, 0, SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小 
			index_flag = true;
		}
		SelectObject(buf_dc, it->bitmap_);
		if (it->trans_flag_){
			TransparentBlt(YGlobalVars::hMdc, it->rect_.left, it->rect_.top, it->rect_.right - it->rect_.left, it->rect_.bottom - it->rect_.top, buf_dc, 0, 0, it->rect_.right - it->rect_.left, it->rect_.bottom - it->rect_.top, RGB(128, 128, 192));
		}
		else{
			BitBlt(YGlobalVars::hMdc, it->rect_.left, it->rect_.top, it->rect_.right - it->rect_.left, it->rect_.bottom - it->rect_.top, buf_dc, 0, 0, SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小
		}
	}
	if (YGlobalVars::flag_result_ == 1){
		YGlobalVars::loginfo.W(__FILE__, 0, YLog::INFO, "compile info", YGlobalVars::compile_info_);
		std::wstring text1;
		if (YGlobalVars::compile_info_ == "28"){//the timeout time was reached.
			text1 = L"请求超时，请检查网络连接。请求超时，请检查网络连接。请求超时，请检查网络连接。";
		}
		else if (YGlobalVars::compile_info_ == "7"){//the timeout time was reached.
			text1 = L"无法连接，服务器异常，请联系QQ：2388946286。";
		}
		else if (YGlobalVars::compile_info_ == "ywrongcmd"){
			text1 = L"程序存在错误命令";
		}
		else{
			text1 = L"WTF";
		}
		DrawText(YGlobalVars::hMdc, text1.c_str(), -1, &YGlobalVars::view_result_rect_, DT_LEFT | DT_WORDBREAK);
	}
	BitBlt(YGlobalVars::hdc, 0, 0, YGlobalVars::window_rect_.right - YGlobalVars::window_rect_.left, YGlobalVars::window_rect_.bottom - YGlobalVars::window_rect_.top, YGlobalVars::hMdc, 0, 0, SRCCOPY);
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
	std::string line = "";
	std::cin >> YGlobalVars::cs_yws_map_h_ >> YGlobalVars::cs_yws_map_w_;
	YGlobalVars::cs_yws_map_.clear();
	for (int i = 0; i<YGlobalVars::cs_yws_map_h_; i++){
		std::vector<int> bufrow;
		for (int j = 0; j<YGlobalVars::cs_yws_map_w_; j++){
			int bufint = 0;
			std::cin >> bufint;
			bufrow.push_back(bufint);
			if (bufint == 10){
				YGlobalVars::cs_yws_map_sr_ = i;
				YGlobalVars::cs_yws_map_sc_ = j;
			}
		}
		YGlobalVars::cs_yws_map_.push_back(bufrow);
	}
	for (int i = 0; i<4; i++){
		YGlobalVars::hMMdc[i] = CreateCompatibleDC(YGlobalVars::hdc);
	}
	HBITMAP bmp;
	bmp = CreateCompatibleBitmap(YGlobalVars::hdc, YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_, YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMMdc[0], bmp);
	DeleteObject(bmp);
	bmp = CreateCompatibleBitmap(YGlobalVars::hdc, YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_, YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMMdc[2], bmp);
	DeleteObject(bmp);
	bmp = CreateCompatibleBitmap(YGlobalVars::hdc, YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_, YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMMdc[1], bmp);
	DeleteObject(bmp);
	bmp = CreateCompatibleBitmap(YGlobalVars::hdc, YGlobalVars::box_w_*YGlobalVars::cs_yws_map_h_, YGlobalVars::box_w_*YGlobalVars::cs_yws_map_w_); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMMdc[3], bmp);
	DeleteObject(bmp);
	HDC buf_dc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC（作为顶级缓冲区）
	for (int i = 0; i<YGlobalVars::cs_yws_map_h_; i++){
		for (int j = 0; j<YGlobalVars::cs_yws_map_w_; j++){
			if (YGlobalVars::cs_yws_map_[i][j] % 10 == 1){
				SelectObject(buf_dc, YGlobalVars::bitmap_info_vector_[10].bitmap_);
			}
			else if (YGlobalVars::cs_yws_map_[i][j] % 10 == 0){
				SelectObject(buf_dc, YGlobalVars::bitmap_info_vector_[5].bitmap_);
			}
			else if (YGlobalVars::cs_yws_map_[i][j] % 10 == 3){
				SelectObject(buf_dc, YGlobalVars::bitmap_info_vector_[16].bitmap_);
			}
			else{
				assert(0 && "WTF");
			}
			BitBlt(YGlobalVars::hMMdc[0], j*YGlobalVars::box_w_, i*YGlobalVars::box_w_, YGlobalVars::box_w_, YGlobalVars::box_w_, buf_dc, 0, 0, SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小
			BitBlt(YGlobalVars::hMMdc[1], i*YGlobalVars::box_w_, (YGlobalVars::cs_yws_map_w_ - 1 - j)*YGlobalVars::box_w_, YGlobalVars::box_w_, YGlobalVars::box_w_, buf_dc, 0, 0, SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小
			BitBlt(YGlobalVars::hMMdc[2], (YGlobalVars::cs_yws_map_w_ - 1 - j)*YGlobalVars::box_w_, (YGlobalVars::cs_yws_map_h_ - 1 - i)*YGlobalVars::box_w_, YGlobalVars::box_w_, YGlobalVars::box_w_, buf_dc, 0, 0, SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小
			BitBlt(YGlobalVars::hMMdc[3], (YGlobalVars::cs_yws_map_h_ - 1 - i)*YGlobalVars::box_w_, j*YGlobalVars::box_w_, YGlobalVars::box_w_, YGlobalVars::box_w_, buf_dc, 0, 0, SRCCOPY);    //采用BitBlt函数贴图，参数设置为窗口大小  
		}
	}
	DeleteDC(buf_dc);
	return;
}
BOOL Game_Init(const HINSTANCE &hInstance, HWND &hwnd, const WNDCLASSEX &wndClass){
	YGlobalVars::InitRectByWH(YGlobalVars::view_rect_, 0, 0, 600, 600);
	YGlobalVars::InitRectByRect(YGlobalVars::view_compiling_rect_, YGlobalVars::view_rect_);
	YGlobalVars::InitRectByWH(YGlobalVars::view_result_rect_, YGlobalVars::view_rect_.left + YGlobalVars::GetRectWidth(YGlobalVars::view_rect_) / 5,
		YGlobalVars::view_rect_.top + YGlobalVars::GetRectHeight(YGlobalVars::view_rect_) / 5,
		YGlobalVars::GetRectWidth(YGlobalVars::view_rect_) * 3 / 5,
		YGlobalVars::GetRectHeight(YGlobalVars::view_rect_) * 3 / 5);
	//YGlobalVars::InitRectByXY(YGlobalVars::view_compiling_rect_,YGlobalVars::view_rect_.left,YGlobalVars::view_rect_.top,YGlobalVars::view_rect_.right,YGlobalVars::view_rect_.bottom);
	YGlobalVars::InitRectByWH(YGlobalVars::btn_content_rect_, YGlobalVars::view_rect_.right, 0, 150, 70);
	YGlobalVars::InitRectByWH(YGlobalVars::btn_start_rect_, YGlobalVars::btn_content_rect_.right, YGlobalVars::btn_content_rect_.top, 150, 70);
	YGlobalVars::InitRectByRect(YGlobalVars::btn_stop_rect_, YGlobalVars::btn_start_rect_);
	//YGlobalVars::InitRectByXY(YGlobalVars::btn_stop_rect_,YGlobalVars::btn_start_rect_.left,YGlobalVars::btn_start_rect_.top,YGlobalVars::btn_start_rect_.right,YGlobalVars::btn_start_rect_.bottom);
	YGlobalVars::InitRectByWH(YGlobalVars::btn_exit_rect_, YGlobalVars::btn_start_rect_.right, YGlobalVars::btn_content_rect_.top, 150, 70);
	YGlobalVars::InitRectByWH(YGlobalVars::code_rect_, YGlobalVars::view_rect_.right, YGlobalVars::btn_content_rect_.bottom, 600, 600);
	YGlobalVars::InitRectByWH(YGlobalVars::window_rect_, 50, 20,
		YGlobalVars::GetRectWidth(YGlobalVars::view_rect_) + YGlobalVars::GetRectWidth(YGlobalVars::code_rect_) + YGlobalVars::window_margin_right_,
		YGlobalVars::GetRectHeight(YGlobalVars::code_rect_) + YGlobalVars::GetRectHeight(YGlobalVars::btn_start_rect_) + YGlobalVars::window_margin_bottom_);
	YGlobalVars::InitRectByWH(YGlobalVars::man_rect_, YGlobalVars::GetRectWidth(YGlobalVars::view_rect_) / 2 - YGlobalVars::box_w_ / 2,
		YGlobalVars::GetRectHeight(YGlobalVars::view_rect_) * 2 / 3 - YGlobalVars::box_w_ / 2, YGlobalVars::box_w_, YGlobalVars::box_w_);
	hwnd = CreateWindow(wndClass.lpszClassName, L"移动迷宫",
		WS_OVERLAPPED | WS_CAPTION, YGlobalVars::window_rect_.left, YGlobalVars::window_rect_.top,
		YGlobalVars::GetRectWidth(YGlobalVars::window_rect_), YGlobalVars::GetRectHeight(YGlobalVars::window_rect_), NULL, NULL, hInstance, NULL);
	YGlobalVars::hdc = GetDC(hwnd);  //获取设备环境句柄
	YGlobalVars::hMdc = CreateCompatibleDC(YGlobalVars::hdc);    //建立兼容设备环境的内存DC（作为二级缓冲区）
	HBITMAP bmp = CreateCompatibleBitmap(YGlobalVars::hdc, YGlobalVars::window_rect_.right - YGlobalVars::window_rect_.left, YGlobalVars::window_rect_.bottom - YGlobalVars::window_rect_.top); //建一个和窗口兼容的空的位图对象
	SelectObject(YGlobalVars::hMdc, bmp);//将空位图对象放到mdc中
	DeleteObject(bmp);
	YGlobalVars::cmd_id_["YGo1"] = 1;
	YGlobalVars::cmd_id_["YGo0"] = 2;
	YGlobalVars::cmd_id_["YTurnL"] = 3;
	YGlobalVars::cmd_id_["YTurnR"] = 4;
	YGlobalVars::cmd_id_["YExit0"] = 5;
	YGlobalVars::cmd_id_["YExit1"] = 6;
	YGlobalVars::cmd_id_["YExit2"] = 7;
	double base_index = 10.0;
	//0-4
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/code.bmp", 0, YGlobalVars::code_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner30.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner01.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner12.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner23.bmp", -1, YGlobalVars::man_rect_, false));
	//5-9
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner_white.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner00.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner11.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner22.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner33.bmp", -1, YGlobalVars::man_rect_, false));
	//10-14
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner_black.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner_e0.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner_e1.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner_e2.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner_e3.bmp", -1, YGlobalVars::man_rect_, false));
	//15-19
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner0123.bmp", -1, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner_trap.bmp", base_index + 0, YGlobalVars::man_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/view.bmp", 0, YGlobalVars::view_rect_, false));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/man.bmp", base_index + 1, YGlobalVars::man_rect_, true));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/btn_start.bmp", 2, YGlobalVars::btn_start_rect_, true));
	//20-24
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/btn_content.bmp", 3, YGlobalVars::btn_content_rect_, true));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/btn_exit.bmp", 4, YGlobalVars::btn_exit_rect_, true));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/btn_stop.bmp", 5, YGlobalVars::btn_stop_rect_, true));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/view_compiling.bmp", base_index + 2, YGlobalVars::view_compiling_rect_, true));
	YGlobalVars::bitmap_info_vector_.push_back(BitmapInfo("data/corner_white.bmp", base_index + 3, YGlobalVars::view_result_rect_, true));
	//25-29
	for (size_t i = 0; i<YGlobalVars::bitmap_info_vector_.size(); i++){
		void * pLoadResource = LoadImage(NULL, string2wstring(YGlobalVars::bitmap_info_vector_[i].path_).c_str(), IMAGE_BITMAP,
			YGlobalVars::GetRectWidth(YGlobalVars::bitmap_info_vector_[i].rect_),
			YGlobalVars::GetRectHeight(YGlobalVars::bitmap_info_vector_[i].rect_), LR_LOADFROMFILE);
		if (!pLoadResource){
			MessageBox(hwnd, string2wstring(YGlobalVars::bitmap_info_vector_[i].path_).c_str(), L"资源初始化失败.", 0); //使用MessageBox函数，创建一个消息窗口
			Game_CleanUp(hwnd);
			return FALSE;
		}
		else{
			YGlobalVars::bitmap_info_vector_[i].bitmap_ = (HBITMAP)pLoadResource;
		}
	}
	return TRUE;
}
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int nShowCmd)
{
	YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WinMain start", "");
	WNDCLASSEX wndClass = { 0 };							//用WINDCLASSEX定义了一个窗口类
	wndClass.cbSize = sizeof(WNDCLASSEX);			//设置结构体的字节数大小
	wndClass.style = CS_HREDRAW | CS_VREDRAW | CS_NOCLOSE;	//设置窗口的样式
	wndClass.lpfnWndProc = WndProc;					//设置指向窗口过程函数的指针
	wndClass.hInstance = hInstance;						//指定包含窗口过程的程序的实例句柄。
	wndClass.hIcon = (HICON)::LoadImage(NULL, L"data/icon.ico", IMAGE_ICON, 0, 0, LR_DEFAULTSIZE | LR_LOADFROMFILE);  //本地加载自定义ico图标
	wndClass.hCursor = LoadCursor(NULL, IDC_ARROW);    //指定窗口类的光标句柄。
	wndClass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);  //为hbrBackground成员指定一个白色画刷句柄	
	wndClass.lpszMenuName = NULL;
	wndClass.lpszClassName = L"class_name";
	if (!RegisterClassEx(&wndClass))				//设计完窗口后，需要对窗口类进行注册，这样才能创建该类型的窗口
		return -1;
	HWND hwnd;
	//Game_Init 放到了ShowWindow前面，不然，没init之前就会触发VM_PAINT了
	if (!Game_Init(hInstance, hwnd, wndClass)){
		return FALSE;
	}
	PlaySound(L"data/仙剑·重楼.wav", NULL, SND_FILENAME | SND_ASYNC | SND_LOOP); //循环播放背景音乐
	//MoveWindow(hwnd,250,80,WINDOW_WIDTH,WINDOW_HEIGHT,true);		//调整窗口显示时的位置，使窗口左上角位于（250,80）处
	ShowWindow(hwnd, nShowCmd);    //调用ShowWindow函数来显示窗口
	UpdateWindow(hwnd);						//对窗口进行更新，就像我们买了新房子要装修一样
	MSG msg = { 0 };
	MapInit(1);
	MapStart();
	while (msg.message != WM_QUIT){
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
		if (GetTickCount() - YGlobalVars::tPre >= 500){
			Game_Paint(hwnd);
		}
		else if (PeekMessage(&msg, 0, 0, 0, PM_REMOVE))   //查看应用程序消息队列，有消息时将队列中的消息派发出去。
		{
			//TranslateMessage( &msg );		//将虚拟键消息转换为字符消息
			DispatchMessage(&msg);			//分发一个消息给窗口程序。
		}
	}
	UnregisterClass(wndClass.lpszClassName, wndClass.hInstance);  //程序准备结束，注销窗口类
	YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WinMain end", "");
	return 0;
}
void ThreadStart(){
	YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "ThreadStart", "");
	YLog logres(YLog::INFO, "data/logres.txt", YLog::OVER);
	YLog logjs(YLog::INFO, "data/logjs.txt", YLog::OVER);
	std::vector<std::string> vec_cs_init;
	std::vector<std::string> vec_cs_user;
	std::vector<std::string> vec_cs_syst;
	CsRawRead("data/raw_cs_syst.txt", vec_cs_syst);
	CsInit(1, vec_cs_init);
	CsRawRead("data/raw_cs_user.txt", vec_cs_user);
	std::string encode_url = UrlEncode(vec_cs_syst) + UrlEncode(vec_cs_init) + UrlEncode(vec_cs_user);
	std::string strTmpStr;
	CURLcode res_code;
	std::string strUrl;
	strUrl = "https://111.230.151.212:85/?type=res&query=" + encode_url;
	res_code = HTTPGet(strUrl, strTmpStr);
	YGlobalVars::flag_compiling_ = 0;
	if (res_code != CURLE_OK){
		logres.W("", 0, YLog::INFO, "Error (CODE)", res_code);
		std::stringstream ss;
		ss << res_code;
		getline(ss, strTmpStr);
		YGlobalVars::compile_info_ = strTmpStr;
		YGlobalVars::flag_start_ = 0;
		YGlobalVars::flag_result_ = 1;
	}
	else{
		logres.W("", 0, YLog::INFO, "RETURN", strTmpStr);
		bool success_flag = true;
		YGlobalVars::cmd_index_ = 0;
		YGlobalVars::cmd_list_.clear();
		std::string bufss = "";
		std::stringstream ss;
		ss << strTmpStr;
		getline(ss, bufss);
		if (bufss[0] == '0'){
			while (getline(ss, bufss)){
				std::stringstream sss;
				std::string bufsss;
				sss << bufss;
				sss >> bufsss;
				if (YGlobalVars::cmd_id_.find(bufsss) == YGlobalVars::cmd_id_.end()){
					YGlobalVars::compile_info_ = "ywrongcmd";
					YGlobalVars::flag_start_ = 0;
					YGlobalVars::flag_result_ = 1;
					success_flag = false;
					break;
				}
				else{
					std::map<int, std::string> bufm;
					bufm[YGlobalVars::cmd_id_[bufsss]] = "";
					YGlobalVars::cmd_list_.push_back(bufm);
				}
			}
		}
		else{
			assert(0 && "error");
		}
		if (success_flag == true){
			YGlobalVars::flag_start_ = 1;
			YGlobalVars::flag_result_ = 0;
			YGlobalVars::pre_t_cmd_ = GetTickCount();
		}
	}
	/*后面可以注释掉*/
	strUrl = "https://111.230.151.212:85/?type=js&query=" + encode_url;
	res_code = HTTPGet(strUrl, strTmpStr);
	if (res_code != CURLE_OK){
		logjs.W("", 0, YLog::INFO, "Error (CODE)", res_code);
	}
	else{
		logjs.W("", 0, YLog::INFO, "RETURN", strTmpStr);
	}
	/**/
	YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "ThreadEnd", "");
	return;
}
LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam){
	/*鼠标在区域内动都会触发处理程序，短按一个按键会触发三次*/
	switch (message){
	case WM_DESTROY:					//若是窗口销毁消息
		//YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WndProc WMDESTROY","");
		Game_CleanUp(hwnd);			//调用自定义的资源清理函数Game_CleanUp（）进行退出前的资源清理
		PostQuitMessage(0);			//向系统表明有个线程有终止请求。用来响应WM_DESTROY消息
		break;									//跳出该switch语句
	case WM_KEYDOWN:					// 若是键盘按下消息
		YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WM_KEYDOWN", YGlobalVars::value2string16(wParam));
		switch (wParam){
		case VK_ESCAPE:
			DestroyWindow(hwnd);
			break;
		case VK_UP:
			if (YGlobalVars::cs_yws_map_dir_ == 0){
				YGlobalVars::cs_yws_map_nr_--;
			}
			else if (YGlobalVars::cs_yws_map_dir_ == 1){
				YGlobalVars::cs_yws_map_nc_++;
			}
			else if (YGlobalVars::cs_yws_map_dir_ == 2){
				YGlobalVars::cs_yws_map_nr_++;
			}
			else {
				YGlobalVars::cs_yws_map_nc_--;
			}
			YGlobalVars::tPre = 0;
			break;
		case VK_LEFT:
			YGlobalVars::cs_yws_map_dir_ += 3;
			YGlobalVars::cs_yws_map_dir_ %= 4;
			YGlobalVars::tPre = 0;
			break;
		case VK_RIGHT:
			YGlobalVars::cs_yws_map_dir_ += 1;
			YGlobalVars::cs_yws_map_dir_ %= 4;
			YGlobalVars::tPre = 0;
			break;
		default:
			break;
		}
		break;									//跳出该switch语句
	case WM_LBUTTONDOWN:
		//YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WM_LMOUSE_x",LOWORD(lParam));
		//YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "WM_LMOUSE_y",HIWORD(lParam));
	{
						   double index = YGlobalVars::JudgeClick(LOWORD(lParam), HIWORD(lParam));
						   YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "click_index", index);
						   if (index == -1){
						   }
						   else if (index == 2){
							   YGlobalVars::flag_start_ = 1;
							   YGlobalVars::flag_compiling_ = 1;
							   YGlobalVars::flag_result_ = 0;
							   YGlobalVars::cmd_list_.clear();
							   std::thread(ThreadStart).detach();
						   }
						   else if (index == 4){
							   DestroyWindow(hwnd);
						   }
						   else{
						   }
	}
		break;
	default:										//若上述case条件都不符合，则执行该default语句
		//YGlobalVars::log1.w(__FILE__, __LINE__, YLog::INFO, "WndProc default","");
		return DefWindowProc(hwnd, message, wParam, lParam);		//调用缺省的窗口过程
	}
	return 0;
}
BOOL Game_CleanUp(HWND hwnd){
	//YGlobalVars::loginfo.W(__FILE__, __LINE__, YLog::INFO, "Game_CleanUp","");
	DeleteObject(YGlobalVars::hPenSolidRed);
	DeleteObject(YGlobalVars::hPenSolidBlack);
	for (size_t i = 0; i<YGlobalVars::bitmap_info_vector_.size(); i++){
		DeleteObject(YGlobalVars::bitmap_info_vector_[i].bitmap_);
	}
	DeleteDC(YGlobalVars::hMdc);
	for (int i = 0; i<4; i++){
		DeleteDC(YGlobalVars::hMMdc[i]);
	}
	ReleaseDC(hwnd, YGlobalVars::hdc);
	return TRUE;
}

