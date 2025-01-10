纯正的使用cmake的项目只需要编写CMakelists.txt文件，不需要编写Makefile（交给cmake来帮助生成）
执行cmake命令即可，不需要执行make命令，但是其实内部可调用make

# 标准流程
mkdir build; cd build  # 创建一个临时目录，这样可以保证【所有的临时文件】都在这里更干净，（如果忘记创建了，可以把CMakeCache.txt删掉后重新来）
cmake .. <option>  # 生成临时文件（包含Makefile，详见如下）；option选项详见如下；
cmake --build . -j8  # 也可以（等价）执行 make -j8
cmake --install .  # 也可以（等价）执行 make install

# 执行完生成的临时文件
*.cmake
Makefile
CMakeCache.txt
CMakeFiles/
……

# CMakelists.txt 格式
命令(参数1 参数2 ...) # 命令是不区分大小写的
project() # 项目名

option(参数名 "参数说明" 是否启用（ON/OFF）)

if(参数a)  # 如果参数a设置的是ON
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")  # 定义/设置CMAKE_CXX_FLAGS变量的值
elseif(NOT 参数b)  # 如果参数b设置的是OFF
  message("xxx")  # 输出信息
else()
  yyyy
endif()

add_executable(binname main.cpp)  # 指定生成的可执行文件

execute_process(COMMAND bash "-c" "cd xxx; echo 123 >wtf ")  # 自定义执行某命令

add_subdirectory(sub_src_dir [, to_dir])  # 项目根目录中指明需要把哪些目录放进来，默认to_dir等于sub_src_dir，就是原封不动把cmake生成的文件放进临时目录（一般用户会新建一个build目录）的对应目录


## CMake的内部变量，无需在CMakelists.txt使用set/option显示定义：
- CMAKE_COMPILER_IS_GNUCC  # 检测编译器是否是gcc，大概率应该是ON
- MSVC  # 检测当前编译器是否为 Microsoft Visual C++ 编译器，大概率应该是OFF


# option选项说明
-DCMAKE_INSTALL_PREFIX=<安装的绝对路径>  # 指定安装路径，这个信息会保存在CMakeCache.txt文件中的CMAKE_INSTALL_PREFIX:PATH
-DCMAKE_BUILD_TYPE=<优化选项>  # Debug（-g） Release（-O3 -DNDEBUG） RelWithDebInfo（-O2 -g -DNDEBUG）


# 其他
生成的文件中flags.make为包含目录