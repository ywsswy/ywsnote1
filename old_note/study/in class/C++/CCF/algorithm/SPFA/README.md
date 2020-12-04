/*
SPFA
判断有无负环：如果某个点进入队列的次数大于等于N次则存在负环（SPFA无法处理带负环的图，可以处理负权边）
SPFA的两种写法，bfs和dfs
SPFA算法有两个优化算法 SLF 和 LLL
SLF：Small Label First 策略，设要加入的节点是j，队首元素为i，若dist(j)<dist(i)，则将j插入队首，否则插入队尾。 
LLL：Large Label Last 策略，设队首元素为i，队列中所有dist值的平均值为x，若dist(i)>x则将i插入到队尾，查找下一元素，直到找到某一i使得dist(i)&lt;=x，则将i出对进行松弛操作。</li><li>引用网上资料，SLF 可使速度提高 15 ~ 20%；SLF + LLL 可提高约 50%。
*/
1）
最短路径
			if(ma2[bufto].len > ma2[head].len + buflen){
				ma2[bufto].len = ma2[head].len + buflen;
				ma2[bufto].from = head;
2）
最小瓶颈路径
			int bufmax = max(ma2[head].len,buflen);
			if(ma2[bufto].len > bufmax){
				ma2[bufto].len = bufmax;
				ma2[bufto].time++;
