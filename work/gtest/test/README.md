
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
ASSERT_EQ(a,值) //失败之后直接退出这个TEST，用于下面会core掉的情况