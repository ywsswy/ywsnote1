# [编译]问题汇总

## 类的成员函数xxx当作普通函数调用，应该改成yyy.xxx
- error: 'xxx' was not declared in this scope

## 未声明就使用类型（非指针）
- error: field 'var1' has incomplete type

## 找不到CLASS类，看头文件是不是没包含

- error: expected ';' before 'CLASS'
- error: 'CLASS' does not name a type
- error: invalid use of incomplete type 'CLASS'
- ISO C++ forbids declaration of *** with no type （这种情况也可能是两个文件互相包含）

##  检查所有被包含的头文件是否有缺失右花括号的
- error: expected '}' at end of input

## 这个类基类里的方法，没有在CLASS中实现
- error: cannot allocate an object of abstract type 'CLASS'

## 检查函数定义是不是忘记写了参数列表
- error: expected primary-expression before


## 套路，included from是包含栈顶在上的，如下问题在<file3>中寻找
g++ -o <xx>.cc
In file included from <file1>:<line1>
				 from <file2>:<line2>
<...>
<file>: note: candidates are:
In file included from <file3>


## 继续套路：
```
In file included from <file_n-1>.h:<行号>,
                 from <file_n-2>.h:<行号>,
                 ...
                 from <file_2>.h:<行号>,
                 from <file_1>.cc<行号>:
<file_n>.h: error: 'X' has not been declared
```
这种头文件依此包含，一层层展开，展开到<file_n>的时候发现了未定义类型，可通过修改前向声明，不要在.h中做include展开来解决

<file>:<行号>: required from here  // 可能是这里导致的错误

## const对象不能调用非const方法（就这一点邪门），const方法里也不能调用非const方法
error: passing ‘const A’ as ‘this’ argument of ‘int A::f2()’ discards qualifiers [-fpermissive]

class A
{
public:
    int f1()
    {
        return 2;
    }
};

int main()
{
    const A b;
    //std::cout << b.f1() << std::endl; // 错误：A类型的const实例不能使用非const方法f1()
    return 0;
}
## declaration of 'Class2 var1' shadows a parameter
var1 已经定义为其他类型的变量了，不能再定义成Class2


## 
<文件1>: In instantiation of <模板类的函数> [with 模板的每个参数类型]
<文件2>:<行号>: required from xxx  // 这是实际调用/使用<文件1>模板类的函数的地方
<文件3>:<行号>: required from yyy  // 这是实际调用/使用<文件2>的地方


# [链接]问题汇总

## 这个类/变量被重复定义，可能是不小心include了它的.cc文件，改成.h即可
.cc:(.text+0x): multiple definition of `CLASS'
.a(.o):.cc:(.text+0x): first defined here

## 类似的，全局变量的定义不能在头文件中，头文件只能声明，否则虽然过了编译阶段，但是不同的.o里面都会有符号表，重定义了
xxx.o: multiple definition of 'var'
yyy.o: previous definition here




# [运行]问题汇总
## $LD_LIBRARY_PATH下找不到动态链接的库（ldd可以看到），要么设置$LD_LIBRARY_PATH，要么改为静态链接 g++ -static -lxxx（会链接libxxx.a）g++ -lxxx（会链接libxxx.so）
./a.out: error while loading shared libraries: libxxx.so.11: cannot open shared object file: No such file or directory

# [configure]
configure: error:  
      *** xxx >= x.y.z is required to build.
环境变量PKG_CONFIG_PATH


make[4]: *** No rule to make target `xxx.cc', needed by `xxx.lo'.  Stop.
因为Makefile.am中写了这个，但是实际不存在这个文件


.deps/xxx.Plo:2: *** missing separator.  Stop.
把.deps删掉重新./configure #？没有根本解决

# [core]问题
很多蜜汁core都是链接问题
1）该链的没有链（例如忘记-lpthread)：就显示打印出来链接语句来检查，bazel --sandbox_debug --subcommands
gcc @文件，其实详细的命令就在这个文件里，
2）重定义的链错了（比如两个库里都有定义，编译的时候不会报错，但是运行就core了；比如两个proto文件中都定义了）core文件中的每一个函数都有可能存在问题