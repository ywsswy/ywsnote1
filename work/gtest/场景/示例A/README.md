备注：
bool FB(int &a)
# 方法一：
## 1.生成CB的hmock类
这里只能用hmock，普通的mock只是多态，只能通过mock对象访问，只能改变本类的函数，改变不了另一个类

## 2.在调用FA之前，按如下进行mock

```
HMockCB mock_cb;
EXPECT_CALL(mock_cb, FB(_)).WillRepeatedly(Return(true));
```
如果不是固定返回，而是触发某自定义的逻辑，则按如下进行mock
```
bool MockFB(int &a) {
...
}
...
HMockCB mock_cb;
或者EXPECT_CALL(mock_cb, FB())).WillRepeatedly(Invoke(MockFB));
```

# 方法二：
## 相比方法一，如果非要用普通mock，那么就把对另一个类的调用单独封装到本类的一个虚函数FC里，然后对本类虚函数FC做mock即可；
缺点是，要改原代码，但是其实最开始设计代码的时候就应该把这种外部依赖单独放在一个函数里，而不是跟本类的逻辑揉在一个大函数里不方便做单测；
