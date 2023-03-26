#include<iostream>
#include<set>
#include<map>
using namespace std;
struct yc4d{
	int len;
	int no;
	int from;
	int to;
};
bool operator < (const yc4d & y1, const yc4d & y2){
	if (y1.len < y2.len){
		return true;
	}
	else if (y1.len == y2.len){
		if (y1.no < y2.no){
			return true;
		}
	}
	return false;
}
int main(){
	//freopen("in2.txt", "r", stdin);
	int gn;           
	int gm;
	cin >> gn >> gm;
	set<yc4d> s;
	int minlen = 0;//总路径长度
	map<int,int> m;//点（0s） 连通分量
	int maxl = -1;//最大连通分量
	int numl = 0;//连通分量0的节点数
	for(int i = 0;i<gn;i++){
		m[i]=-1;
	}
	for(int i = 0;i<gm;i++){
		yc4d buf;
		int bufd;
		buf.no = i;
		cin >> bufd;
		buf.from = bufd-1;
		cin >> bufd;
		buf.to = bufd-1;
		cin >> buf.len;
		s.insert(buf);
	}
	while (!s.empty()){
		set<yc4d>::iterator it = s.begin();
		//路径两端在不同的连通分量中
		if (m[it->from] == -1 || m[it->to] == -1 || m[it->from] != m[it->to]){
			minlen += it->len;
			if(m[it->from] >= 0 && m[it->to] >= 0){
				if(m[it->from] == 0){
					int buf = m[it->to];
					for(unsigned int i = 0;i<m.size();i++){
						if(m[i] == buf){
							m[i] = 0;
							numl++;
						}
					}
				}
				else if(m[it->to] == 0){
					int buf = m[it->from];
					for(unsigned int i = 0;i<m.size();i++){
						if(m[i] == buf){
							m[i] = 0;
							numl++;
						}
					}
				}
				else{
					int buf = m[it->from];
					for(unsigned int i = 0;i<m.size();i++){
						if(m[i] == buf){
							m[i] = m[it->to];
						}
					}
				}
			}
			else if(m[it->from] == -1 && m[it->to] == -1){
				maxl++;
				m[it->from] = maxl;
				m[it->to] = maxl;
				if(maxl == 0){
					numl = 2;
				}
			}
			else if(m[it->from] == -1){
				m[it->from] = m[it->to];
				if(m[it->to] == 0){
					numl++;
				}
			}
			else{//(m[it->to] == -1)
				m[it->to] = m[it->from];
				if(m[it->from] == 0){
					numl++;
				}
			}
		}
		s.erase(it);
		if(numl >= gn){
			break;
		}
	}
	cout << minlen;
	return 0;
}
