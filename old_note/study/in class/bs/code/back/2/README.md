#include <iostream>
#include <string>
#include <fstream>
#include <sstream>
#include <vector>
#include "YLog.h"
#include "curl/curl.h"
#pragma comment(lib,"libcurl.lib")  
static size_t DownloadCallback(char *buffer, size_t sz, size_t nmemb, void *writer){
	std::string* psResponse = (std::string*) writer;
	psResponse->append(buffer, sz * nmemb);
	return sz * nmemb;
}
CURLcode HTTPGet(const std::string &url,const std::string &res){
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
std::string UrlEncode(const std::vector<std::wstring> &vec_cs){
	std::wstring raw_cs = L"";
	for(std::vector<std::wstring>::const_iterator it = vec_cs.begin();it!=vec_cs.end();it++){
		raw_cs += *it;
		raw_cs += L"\r\n";
	}
	std::string result;
	for(unsigned int i = 0; i< static_cast<unsigned int>(raw_cs.length()); i++){
		unsigned char ch = unsigned char(raw_cs[i]);
		if(ch == ' '){
			result += '+';
		}else if(ch >= 'A' && ch <= 'Z'){
			result += ch;
		}else if(ch >= 'a' && ch <= 'z'){
			result += ch;
		}else if(ch >= '0' && ch <= '9'){
			result += ch;
		}else if(ch == '-' || ch == '-' || ch == '.' || ch == '!' || ch == '~' || ch == '*' || ch == '\'' || ch == '(' || ch == ')' ){
			result += ch;
		}else{
			result += "%";
			result += "0123456789abcdef"[unsigned int(ch) / 16];
			result += "0123456789abcdef"[unsigned int(ch) % 16];
		}
	}
	return result;
}
void ReadCsFromFile(const std::string &file_name, std::vector<std::wstring> &vec_cs){
	std::wstring line = L"";
	std::wifstream if1(file_name.c_str());
	while(getline(if1,line)){
		vec_cs.push_back(line);
	}
	return;
}
class YGlobal{
public:
	static std::vector<std::vector<std::vector<int> > > cs_yws_map;
	static std::vector<int> cs_yws_map_h;
	static std::vector<int> cs_yws_map_w;
	static std::vector<int> cs_yws_map_sr;//0s
	static std::vector<int> cs_yws_map_sc;
};
std::vector<std::vector<std::vector<int> > > YGlobal::cs_yws_map;
std::vector<int> YGlobal::cs_yws_map_h;
std::vector<int> YGlobal::cs_yws_map_w;
std::vector<int> YGlobal::cs_yws_map_sr;//0s
std::vector<int> YGlobal::cs_yws_map_sc;
void CsInit(){
	//0
	YGlobal::cs_yws_map_h.push_back(6);
	YGlobal::cs_yws_map_w.push_back(3);
	YGlobal::cs_yws_map_sr.push_back(3);
	YGlobal::cs_yws_map_sc.push_back(1);
	//1
	YGlobal::cs_yws_map_h.push_back(5);
	YGlobal::cs_yws_map_w.push_back(4);
	YGlobal::cs_yws_map_sr.push_back(2);
	YGlobal::cs_yws_map_sc.push_back(1);
	for(int challenge = 0;challenge<2;challenge++){
		std::vector<std::vector<int> > bufmap;
		for(int i = 0;i<YGlobal::cs_yws_map_h[challenge];i++){
			std::vector<int> bufrow;
			for(int j = 0;j<YGlobal::cs_yws_map_w[challenge];j++){
				if(i == 0 || i == YGlobal::cs_yws_map_h[challenge]-1 || j == 0 || j == YGlobal::cs_yws_map_w[challenge]-1){
					bufrow.push_back(1);
				}else{
					bufrow.push_back(0);
				}
			}
			bufmap.push_back(bufrow);
		}
		bufmap[YGlobal::cs_yws_map_sr[challenge]][YGlobal::cs_yws_map_sc[challenge]] = 4;
		YGlobal::cs_yws_map.push_back(bufmap);
	}
}
YLog log1(YLog::INFO, "logjs.txt",YLog::OVER);
void MakeCsHead(const int challenge, std::vector<std::wstring> &vec_cs){
	std::wstring line = L"";
	std::wstringstream ss;
	vec_cs.push_back(L"g_yws_map = []");
	line = L"g_yws_map_w = ";
	ss.clear();
	ss << line << YGlobal::cs_yws_map_w[challenge];
	getline(ss,line);
	vec_cs.push_back(line);
	line = L"g_yws_map_h = ";
	ss.clear();
	ss << line << YGlobal::cs_yws_map_h[challenge];
	getline(ss,line);
	vec_cs.push_back(line);
	vec_cs.push_back(L"GYwsCreateMap = () ->");
	vec_cs.push_back(L"	for i in [0..g_yws_map_h-1]");
	vec_cs.push_back(L"		yws_map_row_data = []");
	vec_cs.push_back(L"		for j in [0..g_yws_map_w-1]");
	vec_cs.push_back(L"			yws_map_row_data[j] = 0");
	vec_cs.push_back(L"		g_yws_map[i] = yws_map_row_data");
	vec_cs.push_back(L"	return");
	vec_cs.push_back(L"GYwsInitMap = () ->");
	for(int i = 0;i<YGlobal::cs_yws_map_h[challenge];i++){
		for(int j = 0;j<YGlobal::cs_yws_map_w[challenge];j++){
			if(YGlobal::cs_yws_map[challenge][i][j] != 0){
				line = L"	g_yws_map[";
				ss.clear();
				ss << line << i << "][" << j << "] = " << YGlobal::cs_yws_map[challenge][i][j];
				getline(ss,line);
				vec_cs.push_back(line);
			}
		}
	}
	vec_cs.push_back(L"	return");
	vec_cs.push_back(L"GYwsCreateMap()");
	vec_cs.push_back(L"GYwsInitMap()");
}
int main(){
	CsInit();
	std::vector<std::wstring> vec_cs;
	MakeCsHead(0,vec_cs);
	ReadCsFromFile("raw_cs.txt",vec_cs);
	std::string encode_url = UrlEncode(vec_cs);
	std::string strUrl = "https://111.230.151.212:85/?type=js2&query=" + encode_url;
	std::string strTmpStr;
	CURLcode res_code = HTTPGet(strUrl,strTmpStr);
	if (res_code != CURLE_OK){
		log1.w("",0,YLog::INFO, "Error (CODE)", res_code);
	} else{
		log1.w("",0,YLog::INFO, "HTTP:", strTmpStr);
	}
	std::stringstream ss;
	ss << strTmpStr;
	std::string bufs;
	while(ss >> bufs){
		std::cout << bufs << '\n';
	}
	return 0;
}

