唯一区别：struct成员默认为public，class默认为private。
继承默认private
因此c++中不像c一样，需要typedef struct，
使用的时候，也不需要struct classname varname = 
就像class一样，classname varname即可。
struct仅含有公有成员时，可以以下赋值：
类 名=(成员1初值,成员2初值...);
union 名{
类 成员;
...
}
各对象成员，不能有自定义的构造函数、析构函数和重载的运算符、不能继承、多态。
无名联合体:(使用时可直接用成员名）
union{
...
}
enum{
GG,
FF,
MM,
}名;

