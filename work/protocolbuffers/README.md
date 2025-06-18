简称：protobuf

- [文档](https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.repeated_field#RepeatedField.erase.details)
- 生成桩代码：
```
protoc --cpp_out=. yws.proto  # 生成c++的桩代码
protoc --go_out=. yws.proto  # 生成golang的桩代码，实际上protc会根据不同的--<language>_out参数来推测生成不同语言的桩代码；这里是会调用protoc-gen-go插件，参考：https://protobuf.dev/reference/go/go-generated/
  -I <PATH>, --proto_path=<PATH>  # proto文件中的import的proto从哪些目录找，可以多次使用该参数，例如`-I ./ -I ./proto`  # 需要在proto文件添加一行 option go_package = 'yyy/xxx' yyy是golang的包路径，xxx是golang的包名写成跟proto的package一样即可
```
- 一个proto文件只会生成一个桩代码文件，所以多个proto文件之间存在import的关系时，需要分别生成桩代码
- 使用：
g++ writer.cc yws.pb.cc -std=c++11 -lprotobuf -lpthread

- 不要用CopyFrom，用operation=操作符，其内也是调用了CopyFrom，是深拷贝，并且推荐操作符能编译报错，避免不同类型之间错误CopyFrom，不用担心浅拷贝；另外MergeFrom是危险的，不同版本的pb文件会造成不断增长的问题；MergeFrom有点6，他会把有值的字段复制过来，无值（默认值）的字段不会复制过来；
- Clear() 清除所有值
- message放内(嵌套)放外，改命名空间，改字段名称，import其他文件的message，生成的二进制都相同

## C++
```
//PB => json_str //还对json_str有要求，必须是空串，不然会从尾巴开始拼接
#include <google/protobuf/util/json_util.h>
std::string json_str;
google::protobuf::util::JsonPrintOptions print_option; //default change to hump & not print default value(always_print_primitive_fields)
print_option.preserve_proto_field_names = true; //use original key
google::protobuf::util::MessageToJsonString(pb, &json_str, print_option);
// json_str => PB//对str的要求：顺序无所谓，【大小写要与pb定义相同】，pb要求string的必须有双引号，要求数值的也可以加双引号，
//pb的定义会禁止一些冲突，例如a_bc和aBc和A_BC是不可以同时存在的，（即去掉下划线，统一大小写后不允许相同），但是json_str却基本都要求必须与pb定义完全相同
google::protobuf::util::JsonParseOptions parse_option; //default strict
parse_option.ignore_unknown_fields = true;
std::string err_info = google::protobuf::util::JsonStringToMessage(std::string, &pb, parse_option).ToString();  // 正常应该是“OK”，特殊的注意事项是空字符串肯定是失败的

// pb序列化
str = .SerializeAsString()
或者
.SerializeToString(&str)
或者
std::fstream output("yin1", std::ios::binary);
.SerializeToOstream(&output);

// 反序列化回pb
bool ParseFromString(const std::string& str)
bool ParseFromArray(const void* data, int size)
ParseFromIstream

// debug
Utf8DebugString()
PrintDebugString()

// 没法随机插入，只能用这种swap的方法
google::protobuf::RepeatedPtrField<std::string> *foo_field(bar.mutable_foo());
for (int i(bar.foo_size() - 1); i > 0; --i)
  foo_field->SwapElements(i, i - 1);
```

## golang 中就是struct/message 和 json的转换（通过json.Marshal/Unmarshal），所以普通struct也是一样的通用写法

```
// => json_str (json库)【不推荐】因为one_of的结构序列化后无法成功反序列化，所以如果发现请求串出现了OneofFilter，就甭想解析了
// 而且pp中的非导出成员（小写字母开头）也是无法被序列化的！！
import "encoding/json"
  var pp ytestp.Ytestma
  jsonRes, err := json.Marshal(pp)  // example: {"request_data":{"filter":{"condition_group_list":[{"condition_list":[{"OneofFilter":{"ValueCompare":{"field_name":"hello"}}}]}]}},"req_option":{"return_limit":22}}
  if err != nil {
    fmt.Printf("json.Marshal failed:%v\n", err)
    return
  }
//json_str =>  (json库)
  err = json.Unmarshal([]byte(testStr), &pp)
  if err != nil {
    fmt.Printf("json.Unmarshal failed:%v\n", err)
    return
  }

// =>json_str (jsonpb库)【推荐】
import "github.com/golang/protobuf/jsonpb"
  jsonRes2, err := (&jsonpb.Marshaler{}).MarshalToString(&pp)  // example: {"requestData":{"filter":{"conditionGroupList":[{"conditionList":[{"valueCompare":{"fieldName":"hello"}}]}]}},"reqOption":{"returnLimit":22}}   //可以看出来区别是少了一层OneofFilter
  if err != nil {
    fmt.Printf("jsonpb MarshalToString failed:%v\n", err)
    return
  }
//json_str => (jsonpb库）注意只能反序列化jsonpb库序列化的字符串
  if err := jsonpb.UnmarshalString(testStr, &pp); err != nil {
    fmt.Printf("UnmarshalString failed:%v", err)
    return
  }

// 序列化，pb的独有写法
import "github.com/golang/protobuf/proto"
  pout, err := proto.Marshal(&pp)
  if err != nil {
    fmt.Printf("%#v\n", err)
    return
  }
  fmt.Printf("%#v\n", pout)
// 反序列化
  err = proto.Unmarshal([]byte(pout), &pp)
  if err != nil { //pout多或少都会err，但是pout为空是成功的
    fmt.Printf("proto.Unmarshal failed")
    return
  }
  fmt.Printf("%v\n", pp)
```

## Python
```
# 序列化
req.SerializeToString())
# pb -> json
from google.protobuf.json_format import MessageToJson
MessageToJson(req)
```