class Test
{
int* p;
}
这时析构函数就不能是trivial的，因为它必须把p申请的内存释放掉！

而如果
class Test
{
int p;
}
析构函数可以什么也不做，就是trivial的。

nontrivial需要你自己负责处理的一些问题，诸如内存的释放



好的程序规范：
仅允许具有平凡析构函数的静态存储期对象，
其他的对象不可以是静态存储期的