MessageToJsonString可以把pb中的any字段输出成json格式
```
syntax = "proto3";
package pp;
import "google/protobuf/any.proto";
message AB{
  int32 x1 = 1;
  int32 x2 = 2;
  google.protobuf.Any x3 = 3;
}
```
输出形如，就是多了一个"@type"字段
```
{
    "x1": 1,
    "x2": 2,
    "x3": {
        "@type": "type.googleapis.com/pp.AB",
        "x1": 11,
        "x2": 22
    }
}
```
同样JsonStringToMessage也能把带有any字段信息（必须有正确的"@type"字段，并且这个type对应的pb桩代码也必须打包进来，否则会报错invalid value Invalid type URL, unknown type）的json反序列化回pb；

如果是Serilize的序列化数据，也能反序列化成功（程序必须把依赖的所有pb桩代码打包进来，否则会报错JsonObjectWriter was not fully closed）