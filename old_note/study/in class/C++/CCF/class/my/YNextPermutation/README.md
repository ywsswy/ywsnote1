#include <iostream>
#include <algorithm>
#include <vector>
template<class T> bool YNextPermutation(T begin, T end){
    if (begin == end){
		return false;
	}
	T before_i = end-1;
	while (before_i != begin){
		T i = before_i;
		if (*--before_i < *i){
			std::reverse(i, end);
			while(!(*before_i < *i)){
				++i;
			}
			std::iter_swap(before_i,i);
			return true;
		}
	}
	std::reverse(begin, end);
	return false;
}
int main() {
	std::vector<int> ve;
	for (int i = 0; i < 4; i++){
		ve.push_back(i);
	}
	do {
		for (int i = 0; i < ve.size(); i++){
			std::cout << ve[i] << ' ';
		}
		std::cout << '\n';
	} while (YNextPermutation(ve.begin(), ve.end()));
	return 0;
}

