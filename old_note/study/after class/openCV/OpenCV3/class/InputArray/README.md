InputArray这个接口类可以是Mat、Mat_<T>、Mat_<T, m, n>、vector<T>、vector<vector<T>>、vector<Mat>。也就意味着当你看refman或者源代码时，如果看见函数的参数类型是InputArray型时，把上诉几种类型作为参数都是可以的。
OutputArray是InputArray的派生类。使用时需要注意的问题和InputArray一样。
