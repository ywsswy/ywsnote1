std::unique_ptr可以支持两个参数，第一个参数A，第二个参数是匿名函数可指明A的析构方法；
如果是希望它帮忙自动管理内存，可以写成：
```
std::unique_ptr<A, void (*)(A*)> p = std::unique_ptr<A, void (*)(A*)>(xxx, [](A* p){ delete p; }};  // 这种定义不支持std::make_unique来赋值；类成员变量初始化默认值时可以写成 = std::unique_ptr<A, void (*)(A*)>(nullptr, [](A* p){ delete p; }};
或者
std::unique_ptr<A> p = std::unique_ptr<A>(xxx);
数组则是
std::unique_ptr<A[], void (*)(A*)> p = std::unique_ptr<A, void (*)(A*)>(xxx, [](A* p) { delete[] p; }};
```

如果是希望手动管理内存（自己负责释放），就可以写成：
```
std::unique_ptr<A, void (*)(A*)> p = std::unique_ptr<A, void (*)(A*)>(xxx, [](A*){});  // 相当于deleter是空，啥都不做
```

例如一个正常的管理流程是：
```
char* p = new char[4];
char* cp = p;
// use cp 需要时刻记得cp不负责析构
delete[] p;
```

应该改成下面的：
```
char* p = new char[4];
std::unique_ptr<char, void(*)(char*)> cp = std::unique_ptr<char, void(*)(char*)>(p, [](char*){});
// use cp 不需要记得，因为看cp的定义就知道cp不负责析构，cp可以修改赋值，使用.reset
delete[] p;
```

更应该改成下面的：
```
std::unique_ptr<char> p = std::unique_ptr<char>(new char[400]);
// use p 不需要记得是否负责析构
std::unique_ptr<char, void(*)(char*)> cp = std::unique_ptr<char, void(*)(char*)>(p.get(), [](char*){});
// use cp 不需要记得是否负责析构
```

或者按照约定，凡是定义成char*的，都不需要负责析构，凡是定义成std::unique_ptr<char>的则可能需要负责析构