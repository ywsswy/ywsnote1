long long YGcd(long long a, long long b){
	while (a != 0){
		b %= a;
		if (b == 0){
			return a;
		}
		else{
			a %= b;
		}
	}
	return b;
}
