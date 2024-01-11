可以不定协议的序列化方式
字段名&结构都能保存在序列化数据中；

这个有个很恶心的坑，就是反序列化的时候，数据不能以'\n'结尾
```
const auto&& root = flexbuffers::GetRoot(reinterpret_cast<uint8_t*>(const_cast<char*>(s.c_str())), s.size());

std::string flex_buff_str;
root.ToString(true, true, flex_buff_str);
```