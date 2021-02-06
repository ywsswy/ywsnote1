.gcov 是编译完生成
.gcda 是运行完写入
所以反复运行的话，.gcda会越来越大


lcov 对 *.gcov 进行改造，生成 *.info
lcov --directory . --capture --output-file t.info
lcov --remove t.info "/usr*" -o t.info
genhtml 把 *.info 生成result文件夹，借助web浏览器，就可以很直观的看到结果
genhtml -o results t.info
//yws_tools.h
#include<iostream>
class YwsTools
{
public:
    YwsTools(){}
    ~YwsTools(){}
    bool IsLeapYear(int y);
};
//yws_tools.cc
#include "yws_tools.h
bool YwsTools::IsLeapYear(int y)
{ //这里不会算覆盖行
    if(y%400 == 0){
        // 是闰年 // 注释不会算覆盖行
        {
            // 函数内多写的花括号不会算覆盖行
        }
        return true;
    }
    else if(y%100 == 0){
        return false;
    }
    else if(y%4 == 0){
        return true;
    }
    else{
        return false;
    }
} //这行不会算覆盖行，仅当文件尾的时候会执行4次
//yws_tools_unittest.cc
class YwsToolsTest: public testing::Test
{
public:
    virtual void SetUp()
    {
        ptr = new YwsTools();
    }

    virtual void TearDown()
    {
        SAFE_DELETE(ptr);
    }
    YwsTools *ptr;

};
TEST_F(YwsToolsTest, test1)
{
    EXPECT_TRUE(ptr->IsLeapYear(2001) == false);
}

TEST_F(YwsToolsTest, test2)
{
    EXPECT_TRUE(ptr->IsLeapYear(2004) == true);
}

TEST_F(YwsToolsTest, test3)
{
    EXPECT_TRUE(ptr->IsLeapYear(2100) == false);
}

TEST_F(YwsToolsTest, test4)
{
    EXPECT_TRUE(ptr->IsLeapYear(2000) == true);
}



EXPECT_TRUE(一个条件)
EXPECT_EQ(a,值)
EXPECT_STREQ(a,字符串)
