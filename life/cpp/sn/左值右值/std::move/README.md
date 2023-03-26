std::move是非一般函数源码实现类似于`A&& MyMove(A& x) { return static_cast<A&&>(x); }`
转为右值类型，但是const属性还可以保留；


常见STL（Standard Template Library）中的移动构造函数行为：
- std::vector 把所有元素都移动走，执行完std::move后是空的；
- std::string 把char都移动走，执行完std::move后是空的，这个性能更好；