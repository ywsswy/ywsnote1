blade build <文件夹>


https://github.com/chen3feng/blade-build

依赖：ninja libstdc++-static




跟bazel区别：
- bazel里面的依赖格式是"//<path>/<to>/<folder>:<obj_name>，例如"//:", "//common:tool"
或者依赖外部的git要在常规格式前面增加"@repository_name"，然后WORKSPACE里面增加git_repository
--sandbox_debug显示详细信息


- blade 
根目录是 BLADE_ROOT
依赖系统的 是写 #pthread
--verbose显示详细信息
编译时是在跟目录，会自动-Ithirdparty -I. -Ibuild64_release （如果加了-p debug参数，则是-Ibuild64_debug）
blade query --deps //test:demo --output-to-dot <file>
testdata = ['//test/test_data/leaf_request.data'], # test.cc中也按照从根目录开始读取
srcs = glob(['*_test.cc']),

## 记一次gtest引入


1) thirdparty/gtest目录下放置“裸”的头文件、lib64_debug文件夹（里面有libgtest.a和libgtest_main.a）（如果不是-p debug，则需要文件夹lib64_release）和一个BUILD
```
cc_library(
    name = 'gtest',
    prebuilt = True  # 表明依赖的libgtest.a已经编译好了
)

cc_library(
    name = 'gtest_main',
    prebuilt = True
)
```
2) BLADE_ROOT里不需要写gtest_libs、和gtest_main_libs，因为会自动去找thirdparty，而不是去系统找

3) 使用时
单测目录里BUILD，都不需要把'//thirdparty/gtest:gtest'加进deps中，cc_test也很智能
xxx.cc
```
#include <gtest/gtest.h>

int Tadd(int a, int b){ 
  return a + b;
}

TEST(Addtest, addtest1) {
  EXPECT_EQ(Tadd(2,4), 6); 
  EXPECT_EQ(Tadd(2,4), 8); 
}
```
4) 运行时
blade test -p debug //<path>/<to>/<folder>:<xxx> --coverage --verbos