#include<iostream>
#include<vector>
#include<queue>
#include<map>
using namespace std;
struct YPoint{
	int x;//客户
	int y;//客户
	int step;//分店：步数 客户：单价
};
bool operator< (const YPoint &p1,const YPoint &p2) {
	if(p1.x<p2.x
			||
		p1.x == p2.x 
		&& 
		p1.y<p2.y
		){
		return true;
	}
	else{
		return false;
	}
}
int n;
int m;
int k;
int d;
int dir[4][2] = {
	{-1,0},
	{0,1},
	{1,0},
	{0,-1},
};
map<YPoint,long long> ke;//step单价 int总价
int findnum = 0;
bool bfs(vector<vector<char> > &v,queue<YPoint> &q);
int main(){
	//freopen("in2.txt","r",stdin);
	cin >> n >> m >> k >> d;
	vector<vector<char> > v;//地图标记 0未走 1已走、分店 2客户
	vector<char> vr;
	for(int i = 0;i<n;i++){
		vr.clear();
		for(int j = 0;j<n;j++){
			vr.push_back(0);
		}
		v.push_back(vr);
	}
	//分店
	queue<YPoint> q; // 步数
	for(int i = 0;i<m;i++){
		int bufx,bufy;
		cin >> bufx >> bufy;
		YPoint bp;
		bp.x = bufx-1;
		bp.y = bufy-1;
		bp.step = 0;
		v[bufy-1][bufx-1] = 1;
		q.push(bp);
	}
	//客户
	for(int i = 0;i<k;i++){
		int bufx,bufy,bufv;
		cin >> bufx >> bufy >> bufv;
		YPoint bp;
		bp.x = bufx-1;
		bp.y = bufy-1;
		bp.step = bufv;
		if(ke.count(bp)){
			bp.step += ke.find(bp)->first.step;
			ke.erase(ke.find(bp));
		}
		ke[bp] = -1;//必须有，因为是插入map
		v[bp.y][bp.x] = 2;
	}
	//禁止
	for(int i = 0;i<d;i++){
		int bufx,bufy;
		cin >> bufx >> bufy;
		v[bufy-1][bufx-1] = 1;
	}
	while(!q.empty()){
		if(bfs(v,q)){
			break;
		}
	}
	long long sum = 0;
	for(map<YPoint,long long>::iterator it = ke.begin();it != ke.end();it++){
		sum+=it->second;
	}
	cout << sum;
	return 0;
}
bool bfs(vector<vector<char> > &v,queue<YPoint> &q){
	YPoint buf;
	YPoint head = q.front();
	q.pop();
	for(int i = 0;i<4;i++){
		int xx = head.x+dir[i][0];
		int yy = head.y+dir[i][1];
		if(xx >= 0
			&&
			xx < n
			&&
			yy >= 0
			&&
			yy < n
			&&
			v[yy][xx] != 1){
			buf.x = xx;
			buf.y = yy;
			buf.step = head.step+1;
			if(v[buf.y][buf.x] == 2){
				map<YPoint,long long>::iterator kit = ke.find(buf);
				kit->second = kit->first.step*buf.step;
				findnum++;
				if(findnum >= k){
					return true;
				}
			}
			v[buf.y][buf.x] = 1;
			q.push(buf);
		}
	}
	return false;
}
