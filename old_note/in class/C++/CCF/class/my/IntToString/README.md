void IntToString(const int &n, std::string &s){
	std::stringstream ss;
	ss << n;
	ss >> s;
	return;
}
