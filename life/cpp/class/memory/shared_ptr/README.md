std::shared_ptr类模板参数只支持一个，但是构造函数是可以支持两个参数，第一个参数A，第二个参数是匿名函数可指明A的析构方法；
如果是希望它帮忙自动管理内存，可以写成：
```
std::shared_ptr<A> p = std::shared_ptr<A>(xxx, [](A* p){ delete p; }};
或者
std::shared_ptr<A> p = std::shared_ptr<A>(xxx);
```

如果是希望手动管理内存（自己负责释放），就可以写成：
```
std::shared_ptr<A> p = std::shared_ptr<A>(xxx, [](A*){});  // 相当于deleter是空，啥都不做
```
