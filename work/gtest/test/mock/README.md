希望调用一个对象的某函数时希望该函数能返回自己指定的结果，
则需要对该对象的类，进行mock类
class MOCK_<class1> : public <class1>
{
public:
  MOCK_METHODx(<fun_name>, <fun_type>);//x表示原始函数的参数个数
}

然后调用mock类的对象的该函数前使用
EXPECT_CALL(该对象,  <fun_name>(::testing::__)).