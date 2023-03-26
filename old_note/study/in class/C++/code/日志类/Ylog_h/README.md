#ifndef _YLOG_H_
#define _YLOG_H_
#include <string>
#include <fstream>
#include <cassert>
#include <ctime>
class Log{
private:
	std::ofstream of;
	int minlevel;
public:
	enum TYPE{ ADD, OVER};
	enum LEVEL{ INFO, ERROR};
	Log(const int level, const std::string &logfile, const int type = Log::OVER): minlevel(level){
		assert((this->ERROR == level || this->INFO == level) && "Logfile create failed, please check the level(Log::ERROR or Log::INFO.");
		if (type == this->ADD){
			this->of.open(logfile.c_str(),std::ios_base::out|std::ios_base::app);
		} else if(type == this->OVER){
			this->of.open(logfile.c_str(),std::ios_base::out|std::ios_base::trunc);
		} else{
			assert(0 && "Logfile create failed, please check the type(Log::OVER or Log::ADD).");
		}
		assert(this->of.is_open() && "Logfile create failed, please check the logfile'name and path.");
		return;
	}	
	~Log(){
		if (this->of.is_open()){
			this->of.close();
		}
		return;
	}
	template<typename T> void w(const std::string &codefile, const int codeline, const int level, const std::string &info, const T &value){
		assert(this->of.is_open());
		if(this->ERROR == level){
			this->of << "[ERROR] ";
		} else if(this->INFO == this->minlevel){
			this->of << "[INFO] ";
		} else{
			assert(0 && "Log write failed, please check the level(Log::ERROR or Log::INFO.");
		}
		time_t sectime = time(NULL);
		tm tmtime;
#ifdef _WIN32
#if _MSC_VER<1600
		tmtime = *localtime(&sectime);
#else
		localtime_s(&tmtime, &sectime);
#endif
#else
		localtime_r(&sectime, &tmtime);
#endif
		this->of << tmtime.tm_year+1900 << '-' << tmtime.tm_mon+1 << '-' << tmtime.tm_mday  << ' ' << tmtime.tm_hour << ':' << tmtime.tm_min << ':' << tmtime.tm_sec << ' ' \
			<< codefile << '(' << codeline << ") " << info << ":\n" << value << std::endl;
		return;
	}
};
#endif /* _YLOG_H_ */

