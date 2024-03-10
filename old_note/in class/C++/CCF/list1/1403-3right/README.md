#include<iostream>
#include<map>
#include<string>
using namespace std;
void final(map<string,string> &mc){
	for(map<string,string>::iterator it = mc.begin();it!=mc.end();it++){
		cout<<it->first<<" ";
		if(it->second != ""){
			cout<<it->second<<" ";
		}
	}
	cout<<endl;
	return;
}
int main(){
	//freopen("in2.txt","r",stdin);
	//建类型，mt：0带参数 1不带参数
	map<char,int> mt;
	string st;
	cin >> st;
	for(int i = 0;i<st.length();i++){
		if(st[i] != ':'){
			if(i!=st.length()-1){
				if(st[i+1] == ':'){
					mt[st[i]] = 0;
				}
				else{
					mt[st[i]] = 1;
				}
			}
			else{
				mt[st[i]] = 1;
			}
		}
	}
	int l;
	cin >> l;
	//读行输出
	string cmd;
	getline(cin,cmd);
	for(int i = 0;i<l;i++){
		cout<< "Case " << i+1 << ": ";
		getline(cin,cmd);
		//定位第一个非空格位置：命令名 并去掉前面空格+命令名
		int loc = 0;
		loc = cmd.find_first_not_of(" ");
		if(loc < 0){
			loc = 0;
		}
		cmd = cmd.substr(loc);
		loc = cmd.find_first_of(" ");
		if(loc < 0){
			//这是个空命令，处理结束
			cout<<endl;
			continue;
		}
		else{
			cmd = cmd.substr(loc);
		}
		int flag = 0;//0等选项 1等参数 2结束改行处理
		map<string,string> mc;
		mc.clear();
		
		string lastcmd;//保存带参数的选项
		while(true){
			//定位第一个非空格位置，并去掉前部分空格
			loc = cmd.find_first_not_of(" ");
			if(loc < 0){
				final(mc);
				break;
			}
			else{
				cmd = cmd.substr(loc);
			}
			//肯定有非空格字符了
		
			string subcmd;
			//存当前命令行的有效参数 mc：""
			//定位第一个空格位置，截取此命令
			loc = cmd.find_first_of(" ");
			if(loc < 0){
				subcmd = cmd;
				cmd = "";
			}
			else{
				subcmd = cmd.substr(0,loc);
				cmd = cmd.substr(loc);
			}
			if(flag == 0){
				//当前状态是等待选项
				if(subcmd.length() == 2
					&&
					subcmd[0] == '-'
					&&
					mt.count(subcmd[1])>0){
						//符合一个选项标准
					if(mt[subcmd[1]] == 0){
						//带参数的选项
						flag = 1;
						lastcmd = subcmd;
					}
					else{
						mc[subcmd]="";
					}
				}
				else{
					//不符合选项标准
					flag = 2;
					final(mc);
					break;
				}
			}
			else{
				//当前状态是等待参数
				mc[lastcmd]=subcmd;
				flag = 0;
			}
		}
	}
	return 0;
}
