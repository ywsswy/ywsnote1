# 最初有
```
Makefile.am
configure.ac
m4/acinclude.m4
```
# 常规流程
- 设置环境变量
- autogen.sh(会调用autoreconf)

|源|过程|目标|
|---|---|---|
|configure.ac|aclocal|aclocal.m4 （宏的集中定义）|
|Makefile.am|automake|Makefile.in|
|-|autoconf|configure|
|-|autoheader|config.h.in|


- configure --prefix= 
|源|过程|目标|ps|
|---|---|---|---|
|X.in (其内可以含有@xxx@）|configure|X （@xxx@会被替换掉）|这里面有config.h|

configure一般会把一些调试信息输出到config.log，方法形如$as_echo "$as_me:${as_lineno-$LINENO}: checking whether $CC accepts -g" >&5
as_echo "$as_me:${as_lineno-$LINENO}:是printf '%s\n' 文件名:代码行数
>&5是输出到config.log
>&6是输出到屏幕了？

- make




# 遇到的问题时有时候改了.cc文件，但是运行却发现没生效，解决方法是，所有有关的.la都应该删掉再编
# .libs下的.a是被其他目录依赖的，.a没了就编译不过了
# .a不会自动生成，仅当有.lo文件改变时，才能让.a重新产生
