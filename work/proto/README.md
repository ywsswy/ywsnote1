[文档](https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.repeated_field#RepeatedField.erase.details)
protoc --cpp_out=. yws.proto  // golang则是 protoc --go_out=. yws.proto
g++ writer.cc yws.pb.cc -std=c++11 -lprotobuf -lpthread

- （不要用CopyFrom）operation=操作符内也是调用了CopyFrom，是深拷贝，并且推荐操作符能编译报错，避免不同类型之间错误CopyFrom，不用担心浅拷贝；另外MergeFrom是危险的，不同版本的pb文件会造成不断增长的问题；
- Clear() 清除所有值
- message放内(嵌套)放外，改命名空间，改字段名称，import其他文件的message，生成的二进制都相同

```
PB test;
//PB => json_str //还对json_str有要求，必须是空串，不然会从尾巴开始拼接
#include <google/protobuf/util/json_util.h>
std::string json_str;
google::protobuf::util::JsonPrintOptions print_option; //default change to hump
print_option.always_print_primitive_fields = true; //also print default value
print_option.preserve_proto_field_names = true; //use original key
google::protobuf::util::MessageToJsonString(pb, &json_str, print_option);
// json_str => PB//对str的要求：顺序无所谓，【大小写要与pb定义相同】，pb要求string的必须有双引号，要求数值的也可以加双引号，
//pb的定义会禁止一些冲突，例如a_bc和aBc和A_BC是不可以同时存在的，（即去掉下划线，同意大小写后不允许相同），但是json_str却基本都要求必须与pb定义完全相同
google::protobuf::util::JsonParseOptions parse_option; //default strict
parse_option.ignore_unknown_fields = true;
std::string err_info = google::protobuf::util::JsonStringToMessage(std::string, &pb, parse_option).ToString();

// pb序列化
SerializeToString(&str)

std::fstream output("yin1", std::ios::out | std::ios::binary);
a.SerializeToOstream(&output);

// 反序列化回pb
ParseFromString(const std::string& str)
bool ParseFromArray(const void* data, int size)

// debug
Utf8DebugString()
PrintDebugString()

// 没法随机插入，只能用这种swap的方法
google::protobuf::RepeatedPtrField<std::string> *foo_field(bar.mutable_foo());
for (int i(bar.foo_size() - 1); i > 0; --i)
  foo_field->SwapElements(i, i - 1);

## golang 中就是struct/message 和 json的转换（通过json.Marshal/Unmarshal），所以普通struct也是一样的通用写法
import "encoding/json"
// => json_str
var pp ytestp.Ytestma
  jsonRes, err := json.Marshal(pp)
  if err != nil {
    fmt.Printf("json.Marshal(pp) failed")
    return
  }
  fmt.Printf("%v\n", string(jsonRes))
//json_str => 
  err = json.Unmarshal([]byte(testStr), &pp)
  if err != nil {
    fmt.Printf("json.Unmarshal failed")
    return
  }
  fmt.Printf("%v\n", pp)

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