vector<int>::iterator it = lower_bound(ve.begin()/first,be.end()/last,value);
获取已升序排列的vector[first,last)左开右闭区间上第一个大于等于value的迭代器位置
upper_bound(ve.begin(),ve.end(),value); //获取第一个大于value的元素位置，
前提都是需要升序排序好后
next_purmutation
iter_swap

