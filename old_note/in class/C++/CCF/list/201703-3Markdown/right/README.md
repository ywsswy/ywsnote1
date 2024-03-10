#include<iostream>
#include<string>
#include<list>
#include<sstream>
using namespace std;
//判断区块类型
int yjudge(string &st){
	if(st[0] == '*'){
		return 3;
	}
	else if(st[0] == '#'){
			return 1;
	}
	else{
		return 2;
	}
}
//处理该区块
void ydeal(int gt,list<string> li){
	for(list<string>::iterator itl = li.begin();itl != li.end();itl++){
		int buflo1 = 0;
		while((buflo1 = (*itl).find_first_of("_")) != -1){
			*itl = (*itl).substr(0,buflo1)
				+"<em>"
				+(*itl).substr(buflo1+1);
			buflo1 = (*itl).find_first_of("_");
			*itl = (*itl).substr(0,buflo1)
				+"</em>"
				+(*itl).substr(buflo1+1);
		}
		while((buflo1 = (*itl).find_first_of("[")) != -1){
			int buflo2 = (*itl).find_first_of("]");
			int buflo3 = (*itl).find_first_of(")");
			*itl = (*itl).substr(0,buflo1)
				+"<a href=\""
				+(*itl).substr(buflo2+2,(buflo3-buflo2-2))
				+"\">"
				+(*itl).substr(buflo1+1,(buflo2-buflo1-1))
				+"</a>"
				+(*itl).substr(buflo3+1);
		}
	}
	if(gt == 1){
		list<string>::iterator itl = li.begin();
		int buflo1 = (*itl).find_first_not_of("#");
		if(buflo1>6){
			buflo1 = 6;
		}
		int buflo2 = (*itl).find_first_not_of(" #");
		stringstream ss;
		ss << buflo1;
		string bufs;
		ss >> bufs;
		*itl = "<h"
			+bufs
			+">"
			+(*itl).substr(buflo2)
			+"</h"
			+bufs
			+">";
	}
	else if(gt == 3){
		for(list<string>::iterator itl = li.begin();itl != li.end();itl++){
			int buflo1 = (*itl).find_first_not_of(" *");
			*itl = "<li>"
				+(*itl).substr(buflo1)
				+"</li>";
		}
		li.push_front("<ul>");
		li.push_back("</ul>");
	}
	else{
		li.front() = "<p>"+li.front();
		li.back() = li.back()+"</p>";
	}
	//输出
	for(list<string>::iterator itl = li.begin();itl != li.end();itl++){
		cout << *itl << endl;
	}
	return;
}
int main(){
	//freopen("in2.txt","r",stdin);
	int gf = 0;//0：等区块，1在区块中
	int gt = 0;//1：标题 2：段落 3：列表
	string gnow;//当前读到的行
	list<string> li;
	while(getline(cin,gnow)){
		if(gf == 1){
			if(gnow.length() == 0){
				gf = 0;
				ydeal(gt,li);
				li.clear();
			}
			else{
				li.push_back(gnow);
			}
		}
		else{
			if(gnow.length() != 0){
				gf = 1;
				li.push_back(gnow);
				gt = yjudge(gnow);
			}
		}
	}
	if(!li.empty()){
		ydeal(gt,li);
	}
	return 0;
}
