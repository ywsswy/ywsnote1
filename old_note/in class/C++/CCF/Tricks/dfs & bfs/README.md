【共同点
两种遍历方式
图，都需要有经过标记
树，不需要经过标记
【不同点
```
//////////////////////////////////////////////////////////////////////
bfs
队列queue/后进后出
可以提前结束，机会等价
获取由顶向下每一点的层数
循环选择性遍历
q.push_back(first);
visit[first] = true;
while(!q.empty()){
	bfs(q);
}
bfs(&q){
	head = q.front();
	q.pop_front();
	while(it in near){
		if(can(it) && !visit(it)){
			visit(it) = true;
			q.push_back(it);
		}
	}
}
1312-5
//////////////////////////////////////////////////////////////////////
dfs
栈statck/后进先出
可以保留相邻（父子）关系
获取自下而上每一点的层数
递归性遍历
s.push_back(first);
visit[first] = true;
while(!s.empty()){
	dfs(s);
}
dfs(&s){
	head = s.back();
	while(it in near){
		if(can(it) && !visit(it)){
			visit(it) = true;
			s.push_back(it);
			return;//如果不写return就不是纯正的dfs，虽然也能成功，但没了父子关系
		}
	}
	s.pop_back();
}
1312-5
201409-4
///////////////////////////////////////////////////////////////
```
循环 重复执行
迭代 按顺序访问列表
遍历 按顺序访问节点
递归 自己调用自身，直到收敛（爆栈）
嵌套 自己调用别人

