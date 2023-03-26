git clone https://github.com/google/googletest.git
切换到稳定分支
cmake .
make && make install


#include "gtest/gtest.h"

TEST(mytest, what)
{
    EXPECT_EQ(1, 1);
}

g++ -E test_ut.cc -std=c++11 -o test_ut.i
# 解析展开宏，查看生成结果

美化格式后可以看到如下内容 astyle -k3 --style=bsd yout.cc #k3表示* ^ &符号挨着右边的word
class mytest_what_Test : public ::testing::Test
{
public:
mytest_what_Test () {} private:
    virtual void TestBody();
    static ::testing::TestInfo *const test_info_ __attribute__ ((unused));
    mytest_what_Test ( mytest_what_Test const &) = delete;
    void operator=( mytest_what_Test const &) = delete;
};
::testing::TestInfo *const mytest_what_Test ::test_info_ = ::testing::internal::MakeAndRegisterTestInfo( "mytest" , "what" , nullptr, nullptr, ::testing::internal::CodeLocation("test_ut.cc", 3), (::testing::internal::GetTestTypeId()), ::testing::internal::SuiteApiResolver< ::testing::Test>::GetSetUpCaseOrSuite("test_ut.cc", 3), ::testing::internal::SuiteApiResolver< ::testing::Test>::GetTearDownCaseOrSuite("test_ut.cc", 3), new ::testing::internal::TestFactoryImpl< mytest_what_Test >);
void mytest_what_Test ::TestBody()
{
switch (0) case 0:
default:
    if (const ::testing::AssertionResult gtest_ar = (::testing::internal::EqHelper::Compare( "1" , "1" , 1 , 1))) ;
    else ::testing::internal::AssertHelper(::testing::TestPartResult::kNonFatalFailure, "test_ut.cc", 5, gtest_ar.failure_message()) = ::testing::Message() ;
}


g++ test_ut.cc -lgtest_main -lgtest -lgcov -lpthread -std=c++11 -fprofile-arcs -ftest-coverage
# gtest_main可以支持代码中不必写main函数直接test
# -fprofile-arcs -ftest-coverage 统计覆盖率，编译链接后会生成*.gcno文件
# ./a.out运行后会生成*.gcda文件
apt install lcov # 包含了genhtml
lcov -c -d . -o lcov.info # 生成代码覆盖信息别忘了要在最开始清理 find . -regextype egrep -regex '.*\.((gcno)|(gcda)|(info))' -type f -exec rm -f {} \;
lcov -r lcov.info "/usr/*" -o lcov.info #移除一些不需要覆盖的目录
genhtml -p $thisdir -o results lcov.info #生成可视化Html