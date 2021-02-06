场景一：希望调用一个对象的某函数时希望该函数能返回自己指定的结果
场景二：对于网络通信等函数，没法真实发起网络通信，所以也可以进行mock，
对某函数进行mock，前提是该函数必须是虚函数！！在开发的时候就应该考虑到后续单测的事情

确认前提后，首先继承改函数所在类，然后写MOCK函数宏
class MOCK_<class1> : public <class1>
{
public:
  MOCK_METHODx(<fun_name>, <fun_type>);//x表示原始函数的参数个数
}

在TEST中调用mock类的对象的该函数前使用
EXPECT_CALL(该对象,  <fun_name>(::testing::__)).<建造器模式写的期待或mock返回值>

次数期待的方法有：
.Times(AtLeast(1))
.Times(<times>) //恰好被调用多少次
mock值的建造器方法有：
.WillOnce(Return(<mock值>))//可以按顺序一直写期望的返回结果
.WillRepeatedly(Return(<mock值>));//始终返回某值，可以跟在WillOnce基础上再建造，反正按顺序判断




class Class1
{
public:
  virtual int Double(int a); //这里必须是虚函数
  int CalDouble(int a);
};

// .cc
int Class1::Double(int a) {
    //net request, please mock me
    return a * 2;
}

int Class1::CalDouble(int a)
{
    return Double(a);
}

// unittest.cc
#include <gtest/gtest.h>
#include <gmock/gmock.h>

class MockClass1 : public Class1
{
public:
    MOCK_METHOD1(Double, int(int));
};

TEST(test1, test2)
{
    MockClass1 c;
    EXPECT_CALL(c, Double(testing::_))
        .Times(testing::AtLeast(1)) //要求至少被调用1次，否则也会报错
        .WillRepeatedly(testing::Return(666)); //某次调用都返回666
    int res = c.CalDouble(3);
    EXPECT_EQ(res, 666);
}