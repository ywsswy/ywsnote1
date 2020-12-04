int yStringToInt(const string &s){
	stringstream ss;
	ss << s;
	int n = 0;
	ss >> n;
	return n;
}
