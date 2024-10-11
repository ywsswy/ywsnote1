纯正的使用cmake的项目只需要编写CMakelists.txt文件，不需要编写Makefile（交给cmake来帮助生成）
执行cmake命令即可，不需要执行make命令，但是其实内部可调用make

# CMakelists.txt 格式
命令(参数1 参数2 ...) # 命令是不区分大小写的
project() # 项目名

# 标准流程
mkdir build; cd build  # 创建一个临时目录，这样可以保证临时文件都在这里更干净，如果忘记创建了，可以把CMakeCache.txt删掉后重新来
cmake .. <option>  # 生成临时文件（包含Makefile，详见如下）；option选项详见如下；
cmake --build . -j8  # 也可以（等价）执行 make -j8
cmake --install .  # 也可以（等价）执行 make install

# 执行完生成的临时文件
*.cmake
Makefile
CMakeCache.txt
CMakeFiles/
……

# option选项说明
-DCMAKE_INSTALL_PREFIX=<安装的绝对路径>  # 指定安装路径，这个信息会保存在CMakeCache.txt文件中的CMAKE_INSTALL_PREFIX:PATH
-DCMAKE_BUILD_TYPE=<优化选项>  # Debug（-g） Release（-O3 -DNDEBUG） RelWithDebInfo（-O2 -g -DNDEBUG）


# 其他
生成的文件中flags.make为包含目录