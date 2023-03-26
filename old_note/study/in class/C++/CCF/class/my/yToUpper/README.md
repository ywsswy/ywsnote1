string的大小写转换
void yToUpper(string &s){
	int len = s.length();
	for(int i = 0;i<len;i++){
		s[i] = toupper(s[i]);//tolower
	}
	return;
}
