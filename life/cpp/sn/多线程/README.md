异步
我见过的写法
一、
std::future


二、
C++11
1)启动线程
#include<thread>
fun1(int pa1){}
std::thread var1(fun1,pa1);
2)
线程启动后，一定要在thread var1销毁前，确定以何种方式等待线程执行结束。
C++11有两种方式来等待线程结束
detach方式，启动的线程自主在后台运行，当前的代码继续执行，写法如std::thread(ThreadStart).detach();
join方式，阻塞主线程，等待启动的线程完成，才会继续往下执行
3)函数传参
正常同上，传引用如下
fun2(int &pa2){}
std::thread var2(fun2,std::ref(pa2));
4)异常情况下等待线程完成
detach方式在创建thread的实例后立即调用detach，这样即使出现了异常thread的实例被销毁，仍然能保证线程在后台运行。
但线程以join方式运行时，需要在主线程的合适位置调用join方法，如果调用join前出现了异常，thread被销毁，线程就会被异常所终结。为了避免异常将线程终结，或者防止线程在函数退出后访问局部变量，就要保证要在函数退出前调用join。
资源获取即初始化：
class thread_guard
{
    thread &t;
public :
    explicit thread_guard(thread& _t) :
        t(_t){}
    ~thread_guard()
    {
        if (t.joinable())
            t.join();
    }
    thread_guard(const thread_guard&) = delete;
    thread_guard& operator=(const thread_guard&) = delete;
};
void func(){
    thread t([]{
        cout << "Hello thread" <<endl ;
    });
    thread_guard g(t);//函数退出时，局部变量g调用其析构函数销毁，从而能够保证join一定会被调用
}
