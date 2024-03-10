#define YINDEX(x) (x-1)
#include<iostream>
#include<iomanip>
#include<string>
#include<sstream>
#include<fstream>
#include<vector>
#include<list>
#include<map>
#include<algorithm>
#include<cmath>
class node{
public:
	node();
	int link;
	int state;
	std::vector<int> ve;
	int size;
	int index;
};
node::node(){
	this->index = 0;
	this->link = 0;
	this->state = 0;
	this->size = 0;
}
int main() {
    std::cin.tie(0);
	int nn = 0;
	int p = 0;
	std::cin >> nn >> p;
	{
		std::string trash = "";
		getline(std::cin,trash);
	}
	for(int ii = 0;ii<nn;ii++){
		int g_finish = 0;
		int g_undeal = 0;
		std::vector<node> ve;
		ve.resize(p,node());
		for(int i = 0;i<p;i++){
			std::string bufstr = "";
			getline(std::cin,bufstr);
			while(1){
				size_t loc = bufstr.find_first_of("RS",0);
				if (loc == std::string::npos) {
					break;
				} else {
					ve[i].size++;
					if(bufstr[loc] == 'R'){
						ve[i].ve.push_back(2);
					} else {//if(ch == 'S'){
						ve[i].ve.push_back(1);
					}
					bufstr = bufstr.substr(loc+1);
					std::stringstream ss;
					ss << bufstr;
					int bufint = 0;
					ss >> bufint;
					ve[i].ve.push_back(bufint);
				}
			}
			if(ve[i].size>0){
				g_undeal++;
				ve[i].state = 0;
			}else{
				g_finish++;
				ve[i].state = -1;
			}
		}
		//开始处理
		int now = 0;
		while(g_undeal > 0){
			if(ve[now].state !=0){
				now = (now+1)%p;
				continue;
			}
			//state == 0->未处理
			if(ve[now].index == ve[now].size){
				ve[now].state = -1;
				g_finish++;
				g_undeal--;
				continue;
			}
			//index < size->看下一个命令
			//只要把state改为1或2，index就++，此处即可直接处理index处代码
			int linkbuf = ve[now].ve[ve[now].index*2 + 1];//0s
			ve[now].link = linkbuf;
			if(ve[now].ve[ve[now].index*2] == 2){
				ve[now].index ++;
				if(ve[linkbuf].state == 1 && ve[linkbuf].link == now){//匹配上对方
					ve[linkbuf].state = 0;
					g_undeal++;
					continue;
				}else{//没有匹配，进入等待
					ve[now].state = 2;
					g_undeal--;
					continue;
				}
			}else{
				ve[now].index ++;
				if(ve[linkbuf].state == 2 && ve[linkbuf].link == now){//匹配上对方
					ve[linkbuf].state = 0;
					g_undeal++;
					continue;
				}else{//没有匹配，进入等待
					ve[now].state = 1;
					g_undeal--;
					continue;
				}
			}
		}
		//判断死锁
		if(g_finish == p){//未发生死锁
			std::cout << 0 << '\n';
		}else{
			std::cout << 1 << '\n';
		}
	}
    return 0;
}

