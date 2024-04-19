https://developers.google.com/protocol-buffers/docs/reference/go-generated#oneof
```
oneof example_name {
    int32 foo_int = 4;
    string foo_string = 9;
    ...
}
生成的c++桩代码中：
enum ExampleNameCase {
  kFooInt = 4,
  kFooString = 9,
  EXAMPLE_NAME_NOT_SET = 0
}

对应的API
- ExampleNameCase example_name_case(); # 判断哪个字段被设置了
- void clear_example_name();
```



c++ json打印出来，就像一个map，所以可以当成把oneof这一层括号去掉，里面只存在一个key-value