scons -j 32 # 32个线程编译

## 注意
scons智能的判断是否需要重新编译哪些文件
首先修改时间没变的一定不重新编译，
修改时间变的文件内容md5不变也不重新编译。
linux下的文件，在windows中的source in sight编译过后文件修改时间依然不变。。。所以这是个bug


# 安装（前提是系统的python默认要打开的是Python3版本）
python -m pip install --user scons


SConscript中

env.aDirs('<subdirectory>') #指定子目录，然后就会去子目录下找SConscript，执行

子目录的SConscript中
Import('env')
env = env.Clone() #即可继承父目录的设置?


#!/usr/bin/env python3

parse_flags = '-g -O0 -std=c++11'
parse_flags = parse_flags + ' ' + '-I/usr/include'
parse_flags = parse_flags + ' ' + '-I /mnt/d/software/'
parse_flags = parse_flags + ' ' + '-lpthread'
parse_flags = parse_flags + ' ' + '-I /mnt/d/software/libconfig/include/ -L /mnt/d/software/libconfig/lib/ -static -lconfig'
parse_flags = parse_flags + ' ' + '-DYWTACH'
Program('stepA', 'stepA.cc', parse_flags=parse_flags)

env = Environment(CCFLAGS="-fdiagnostics-color") #-fdiagnostics-color=always
env.Program(source="foo.c")

env.MergeFlags({'CXXFLAGS':['-fdiagnostics-color=always']}) #这个是自动去重了的