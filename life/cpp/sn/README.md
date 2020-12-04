# 随笔
* 不要返回函数内部定义的引用，因为结束后会释放
* 即使#ifndef已经避免重复包含了，声明和定义分开写到两个文件，是因为#ifndef只能保证对于一个cpp内不会重复包含，如果多个cpp都包含了还是会冲突
* vs2010不支持std::thread，不支持直接写{}隐示构造结构体

所有map，不要用[]这种未定义行为访问，都是先find再操作

for (auto &&it : container) # 这么访问咯

避免裸指针，用auto ptr = std::make_shared<CLASS>();

模板函数是不能把实现写到cpp里的，头文件中就要写