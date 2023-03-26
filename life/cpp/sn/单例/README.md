构造函数是private的
写一个public的
static <CLASS>& GetInstance() {
  static <CLASS> instance;
  return instance;
}



#ifndef DISALLOW_COPY_AND_ASSIGN
#define DISALLOW_COPY_AND_ASSIGN(TypeName) \
    TypeName(const TypeName&); \
    TypeName& operator=(const TypeName&)
#endif


// 饿汉和懒汉
// 懒汉是不到万不得已不去做（用到的时候才去判断是否是NULL，然后再new 实例）
// 饿汉是定义类的的时候就上来new好了