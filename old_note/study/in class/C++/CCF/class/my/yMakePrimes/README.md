//线性筛素数 小于等于maxs的素数都存到vep1中(0s) 
void yMakePrimes(vector<int> &vep1,int maxs) {
	int count = 0;
	vector<int> vev1;//1:被筛掉(1s) 
	for(int i = 0;i<=maxs;i++){
		vev1.push_back(0);
	}
	for(int i = 2;i<maxs; i++) {//倍数 
		if(!vev1[i]){//未被筛掉就是素数 
			vep1.push_back(i);
			count++;//素数个数 
		}
		//遍历所有已求出的素数，在当前倍数下筛掉合数 
		for(int j = 0;j<count;j++){ 
			int bufi = i*vep1[j];
			if(bufi > maxs){
				break;
			}
			else{
				vev1[bufi] = 1;//筛掉 
			}
			if(i%vep1[j] == 0){//保证每个合数只会被它最小的质因子筛掉
				break;
			}
		}
	}
	if(!vev1[maxs]){
		vep1.push_back(maxs);
	} 
	return;
}
