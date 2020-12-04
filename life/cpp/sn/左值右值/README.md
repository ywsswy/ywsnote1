A a = std::move(b); //b是const //这种写法会让b失效的
* 【左值】是可取地址的表达式结束后依然存在的持久对象，【右值】是临时的
* 【左值引用】和【右值引用】都是引用类型/别名。无论是声明一个左值引用还是右值引用，都必须立即进行初始化。
* 常量左值引用是个“万能”的引用类型。它可以接受非常量左值、常量左值、右值对其进行初始化。

## 1
error: cannot bind ‘int’ lvalue to ‘const int&&’
```
int main()
{
    int a = 3;
    const int &&b = a;
    return 0;
}
```
## 2
error: invalid initialization of non-const reference of type 'std::shared_ptr<>&' from an rvalue of type 'std::shared_ptr<>'
```
std::shared_ptr<> &rr = std::make_shared<>();//应该改成&&
```


const 对性能无所谓，关键是引用要用对

迭代器就以下三种使用
auto &&it : const Class 得到 const &
auto &&it : Class 得到 &
const auto &it : Class 得到 const &