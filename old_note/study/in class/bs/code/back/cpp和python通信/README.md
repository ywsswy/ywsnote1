#include <iostream>
#include <string>
#include <fstream>
#include <sstream>
#include <vector>
#include "YLog.h"
#include "curl/curl.h"
#pragma comment(lib,"libcurl.lib")  
YLog log1(YLog::INFO, "log1.txt",YLog::OVER);
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
int main(){
	std::vector<std::wstring> vec_cs;
	ReadCsFromFile("raw_cs.txt",vec_cs);
	std::string encode_url = UrlEncode(vec_cs);
	std::string strUrl = "https://111.230.151.212:85/?query=" + encode_url;
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
