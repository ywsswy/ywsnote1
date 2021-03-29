## golang(proto2) 对应的字段类型，命名都会变成首字母大写的驼峰
|-|(生成的go的struct里面的类型)|GetX()|
|---|---|---|
|optional int32 a = 1|A *int32. 不太对，好像又不是指针|func (m *Class) GetA() int32|
|optional Info b = 2|B *Info|func (m *Class) GetB() *Info|
|repeated int32 c = 3|C []int32|func (m *Class) GetC() []int32|
|optional bytes f = 7|F []byte|
golang还会在生成的type <messange> struct里面最后加三个字段XXX_

## c++(proto3) 对应的生成方法
|-|clear_x()|x()|set_x()|has_x()|release_x()|mutable_x()|set_allocated_x()|x_size()|add_x()|
|---|---|---|---|---|---|---|---|---|---|
|int32 a = 1|void clear_a()|int32 a() const|void set_a(int32 value)||||||
|Info b = 2|void clear_b()|const Info& b() const||bool has_b() const<br>{"b":{}}转成pb后，has_b()为true<br>{}转成pb后，has_b()为false|Info* release_b()<br>执行后has_b()变成false|Info* mutable_b()|void set_allocated_b(Info* b)||
|repeated int32 c = 3|void clear_c()|const google::protobuf::RepeatedField<int32>& c() const|void set_c(int index, int32 value)|||google::protobuf::RepeatedField<int32 >* mutable_c()||int c_size() const|void add_c(int32 value)|
|-|-|int32 c(int index) const|||||||
|repeated Info d = 4|void clear_d()|const Info& d(int index) const||||Info* mutable_d(int index)|-|int d_size() const|Info* add_d()|
|-|-|const google::protobuf::RepeatedPtrField<Info>& d() const||||google::protobuf::RepeatedPtrField< Info >* mutable_d()|||
|map<int32, int32> e = 5|void clear_e()|const google::protobuf::Map<int32,int32>& e() const|-|-|-|google::protobuf::Map<int32,int32>* mutable_e()|-|int e_size() const|-|
|string f = 6<br>bytes f = 7<br>看起来接口一样，但json化不同|void clear_f()|const std::string& f() const|void set_f(const std::string& value)|-|std::string* release_f()|std::string* mutable_f()|void set_allocated_f(std::string* f)|
|-|-|-|<font color=red>void set_f(std::string&& value)|
|-|-|-|void set_f(const char* value)|
|-|-|-|void set_f(const char* value, size_t size)|
|repeated string h = 8<br>repeated bytes h = 9|void clear_h()|const std::string& h(int index) const<br><font color=red>是否带\0|void set_h(int index, const std::string& value)|-|-|std::string* mutable_h(int index)|-|int h_size() const|std::string* add_h()|
|-|-|const google::protobuf::RepeatedPtrField<std::string >& h() const|void set_h(int index, std::string&& value)|-|-|google::protobuf::RepeatedPtrField<std::string >* mutable_h()|-|-|void add_h(const std::string& value)|
|-|-|-|void set_h(int index, const char* value)|-|-|-|-|-|void add_h(std::string&& value)|
|-|-|-|void set_h(int index, const char* value, size_t size)|-|-|-|-|-|void add_h(const char* value)|
|-|-|-|-|-|-|-|-|-|void add_h(const char* value, size_t size)|




```
syntax = "proto3";
package my_space;
import "other.proto";
message Yws {
  enum TYPE1 {
    AND = 0;
    OR = 1;
  }
}
enum TYPE2 {
  AND = 0;
  OR = 1;
}

// 生成c++后：
Yws_TYPE1_AND // 0
TYPE2::AND // 0
```


其他类型
google.protobuf.Any details = 2;

方法：
bool PackFrom(const google::protobuf::Message& message);
bool UnpackTo(google::protobuf::Message* message) const;
看起来发送方所有类型都能pack到这个字段里，一般还会有个字段标识类型，接收方再根据类型标识，写if-else来Unpack