## c++(proto3) 对应的生成方法
|-|golang(proto3)对应的struct字段类型<br>命名都会变成首字母大写的驼峰<br>所以无需set/get方法|clear_x()|x()|set_x()|has_x()|release_x()|mutable_x()|set_allocated_x()|x_size()|add_x()|
|---|---|---|---|---|---|---|---|---|---|---|
|int32 a = 1|A int32|void clear_a()|int32 a() const|void set_a(int32 value)||||||
|Info b = 2|B *Info|void clear_b()|const Info& b() const||bool has_b() const<br>{"b":{}}转成pb后，has_b()为true<br>{}转成pb后，has_b()为false|Info* release_b()<br>执行后has_b()变成false|Info* mutable_b()|void set_allocated_b(Info* b)||
|repeated int32 c = 3|C []int32|void clear_c()|const google::protobuf::RepeatedField<int32>& c() const|void set_c(int index, int32 value)|||google::protobuf::RepeatedField<int32 >* mutable_c()||int c_size() const|void add_c(int32 value)|
|-|-|-|int32 c(int index) const|||||||
|repeated Info d = 4|D []*Info|void clear_d()|const Info& d(int index) const||||Info* mutable_d(int index)|-|int d_size() const|Info* add_d()|
|-|-|-|const google::protobuf::RepeatedPtrField<Info>& d() const||||google::protobuf::RepeatedPtrField< Info >* mutable_d()|||
|map<int32, int32> e = 5|E map[int32]int32|void clear_e()|const google::protobuf::Map<int32,int32>& e() const|-|-|-|google::protobuf::Map<int32,int32>* mutable_e()|-|int e_size() const|-|
|string f = 6<br>bytes f = 7<br>看起来c++接口一样，但json化不同|F string<br>F []byte|void clear_f()|const std::string& f() const|void set_f(const std::string& value)|-|std::string* release_f()|std::string* mutable_f()|void set_allocated_f(std::string* f)|
|-|-|-|<font color=red>void set_f(std::string&& value)|
|-|-|-|void set_f(const char* value)|
|-|-|-|void set_f(const char* value, size_t size)|
|repeated string g = 8<br>repeated bytes g = 9|G []string<br>G [][]byte|void clear_g()|const std::string& g(int index) const<br><font color=red>是否带\0|void set_g(int index, const std::string& value)|-|-|std::string* mutable_g(int index)|-|int h_size() const|std::string* add_g()|
|-|-|-|const google::protobuf::RepeatedPtrField<std::string >& g() const|void set_g(int index, std::string&& value)|-|-|google::protobuf::RepeatedPtrField<std::string >* mutable_g()|-|-|void add_g(const std::string& value)|
|-|-|-|void set_g(int index, const char* value)|-|-|-|-|-|void add_g(std::string&& value)|
|-|-|-|void set_g(int index, const char* value, size_t size)|-|-|-|-|-|void add_g(const char* value)|
|-|-|-|-|-|-|-|-|-|void add_g(const char* value, size_t size)|




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

// 生成golang后：
type Yws_TYPE1 int32
const (
	Yws_AND Yws_TYPE1 = 0
)
type TYPE2 int32
const (
	TYPE2_AND TYPE2 = 0
)
```


其他类型
google.protobuf.Any details = 2;

方法：
bool PackFrom(const google::protobuf::Message& message);
bool UnpackTo(google::protobuf::Message* message) const;
看起来发送方所有类型都能pack到这个字段里，一般还会有个字段标识类型，接收方再根据类型标识，写if-else来Unpack