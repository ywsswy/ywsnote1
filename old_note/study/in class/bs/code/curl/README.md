#include <iostream>
#include <string>
#include <fstream>
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
int main()
{
	std::string strUrl = "http://111.230.151.212:85/?query=woyehao";
	std::string strTmpStr;
	CURLcode res_code = HTTPGet(strUrl,strTmpStr);
	if (res_code != CURLE_OK){
		log1.w("",0,YLog::INFO, "Error (CODE)", res_code);
	} else{
		log1.w("",0,YLog::INFO, "HTTP:", strTmpStr);
	}
	return 0;
}
