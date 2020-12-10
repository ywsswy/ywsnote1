核心是有一个原始C++文件
再写一个h文件，（只做封装，并且使用如下方法把封装的方法提供给别人调用
#ifdef __cplusplus
extern "C" {
#endif

两种提供（so、.h + .c/.o）


# --file1-- 
```
#ifndef YLOG_WRAPPER_H_ 
#define YLOG_WRAPPER_H_
#ifdef __cplusplus
extern "C" {
#endif
struct YLogWrapper
{
    //内部c++类隐藏起来，隐藏的实现里面会做强转
    void *internal_; 
    //对外只暴露封装的接口，这样函数指针的写法仅仅是为了外部使用上有类似“类”的错觉
    //接口的第一个参数始终都是调用方对象本身
    void (*W)(const struct YLogWrapper *log, const char *codefile, const int codeline, const int level, const char *info, const char *value);
};
//提供new和delete的接口
extern struct YLogWrapper *NewYLog(const int level, const char *logfile, const int type);
extern void DeleteYLog(struct YLogWrapper *log);
#ifdef __cplusplus
};
#endif
#endif
```

# --file2--
```
// 这个文件是用c++编译的
// ... 各种c++写法
#include "ylog_wrapper.h"
#ifdef __cplusplus
extern "C" {
#endif

//用静态类的函数当作函数指针给到外部struct的接口
class GlobalFunction
{
public:
    static void W(const struct YLogWrapper *log, const char *codefile, const int codeline, const int level, const char *info, const char *value)
    {   
        if (log != NULL)
        {   
            YLog *ylog = (YLog *)(log->internal_);
            ylog->W(codefile, codeline, level, info, value);
        }
    }
};
struct YLogWrapper *NewYLog(const int level, const char *logfile, const int type)
{
    struct YLogWrapper *log = new struct YLogWrapper;
    log->W = GlobalFunction::W;
    log->internal_ = new YLog(level, logfile, type);
    return log;
}
void DeleteYLog(struct YLogWrapper *log)
{
    if (log != NULL)
    {   
        YLog *ylog = (YLog *)(log->internal_);
        delete ylog;
        ylog = NULL;
        delete log;
        log = NULL;
    }
}
#ifdef __cplusplus
};
#endif
```
# --file3--
```
// 这个文件是用c编译的
#include "ylog_wrapper.h"
#include <stdio.h>

int main(void)
{
    struct YLogWrapper *ylog1 = NewYLog(1, "test.txt", 0); 
    ylog1->W(ylog1, __FILE__, __LINE__, 0, "test1", "value1");
    ylog1->W(ylog1, __FILE__, __LINE__, 1, "test1", "value2");
    ylog1->W(ylog1, __FILE__, __LINE__, 2, "test1", "value3");
    DeleteYLog(ylog1);
    return 0;
}
```