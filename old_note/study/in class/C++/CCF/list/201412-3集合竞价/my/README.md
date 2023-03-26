#include<iostream>
#include<string>
#include<vector>
#include<sstream>
#include<map>
using namespace std;
struct yclass{
	string cmd;
	int flag;//0:unuseful 1:useful
};
int main(){
	freopen("in2.txt","r",stdin);
	string bufs;
	vector<yclass> v;
	while(getline(cin,bufs)){
		yclass buf;
		buf.cmd = bufs;
		buf.flag = 1;
		v.push_back(buf);
	}
	for(int i = 0;i<v.size();i++){
		bufs = v[i].cmd;
		int bufl = bufs.find_first_of(" ");
		bufs = v[i].cmd.substr(0,bufl);
		if(bufs == "cancel"){
			bufs = v[i].cmd.substr(bufl+1);
			stringstream ss;
			ss<<bufs;
			ss>>bufl;
			v[bufl-1].flag = 0;
			v[i].flag = 0;
		}
	}
	map<double,long long> buy;
	map<double,long long> sell;
	for(int i = 0;i<v.size();i++){
		bufs = v[i].cmd;
		int bufl = bufs.find_first_of(" ");
		bufs = v[i].cmd.substr(0,bufl);
		if(v[i].flag == 1){
			string bufs1;
			string bufs2;
			stringstream ss;
			double bufd;
			long long bufll;
			if(bufs == "buy"){
				bufs = v[i].cmd.substr(bufl+1);
				bufl = bufs.find_first_of(" ");
				bufs1 = bufs.substr(0,bufl);
				bufs2 = bufs.substr(bufl+1);
				ss<<bufs1;
				ss>>bufd;
				ss.clear();
				bufd = -bufd;
				ss<<bufs2;
				ss>>bufll;
				if(buy.find(bufd) != buy.end()){
					buy[bufd] += bufll;
				}
				else{
					buy[bufd] = bufll;
				}
			}
			else if(bufs == "sell"){
				bufs = v[i].cmd.substr(bufl+1);
				bufl = bufs.find_first_of(" ");
				bufs1 = bufs.substr(0,bufl);
				bufs2 = bufs.substr(bufl+1);
				ss<<bufs1;
				ss>>bufd;
				ss.clear();
				ss<<bufs2;
				ss>>bufll;
				if(sell.find(bufd) != sell.end()){
					sell[bufd] += bufll;
				}
				else{
					sell[bufd] = bufll;
				}
			}
		}
	}
	double resd = 0;
	long long resl = 0;
	long long bl;
	long long sl;
	double bd;
	double sd;
	map<double,long long>::iterator itb = buy.begin();
	map<double,long long>::iterator its = sell.begin();
	bd = -itb->first;
	bl = itb->second;
	sd = its->first;
	sl = its->second;
	while(true){
		if(bd < sd){
			break;
		}
		else{
			resd = bd;
			if(sl >= bl){
				resl += bl;
				sl -= bl;
				bl = 0;
			}
			else{
				resl += sl;
				bl -= sl;
				sl = 0;
			}
		}
		if(sl == 0){
			its++;
			if(its == sell.end()){
				break;
			}
			else{
				sd = its->first;
				sl = its->second;
			}
		}
		if(bl == 0){
			itb++;
			if(itb == buy.end()){
				break;
			}
			else{
				bd = -itb->first;
				bl = itb->second;
			}
		}
	}
	cout.setf(ios::fixed);
	cout.precision(2);
	cout << resd << " " << resl;
	return 0;
}
