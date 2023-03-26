int main(){
	std::locale china("chs");
	std::wifstream wif("test.txt");
	wif.imbue(china);
	std::wstring ws3;
	wif >> ws3;
	
	std::wstring ws = L"字";
	return 0;
}

