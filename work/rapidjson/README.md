#CLASS
## 每个 JSON 值都储存为 Value 类，而 Document 类则表示整个 DOM，它存储了一个 DOM 树的根 Value
rapidjson::Document b1(rapidjson::kObjectType); //构造函数中可以指定类型，还有kArrayType
.Parse(std::string) # 把字符串解析成dom，存在自身中
.IsObject()
.HasMember(std::string) //或者更高效的Value::ConstMemberIterator itr = document.FindMember("hello");判断(itr != document.MemberEnd())// itr->value 类似it->second //itr->name 类似it->first
.MemberBegin()
.IsArray()
.IsInt()
.IsDouble()
.AddMember()
.GetInt() # 这是干啥的
Value& [<name/num(0s)>] #是可以进行比较的(document["i"] != 123)
.IsNull()

# USAGE
## 通过字符串路径返回dom的一个节点指针
rapidjson::Value *hit_value = rapidjson::Pointer("/hits/hits").Get(document);


# 经验：
- 推测一个document析构的时候，会把其allocator创建的内存析构掉，所以要根据生命周期考虑该使用谁的GetAllocator。
- Value 和 Document之间转换，没有public直接赋值，但是AddMember会把Document 当成GenericValue，AddMember内部并没有深拷贝，只有指针传递，所以确保AddMember的任何参数生命周期都是安全的，《包括字符串key》，固定大小的 JSON 类型（Number、True、False、Null）除外
- doc.AddMember(rapidjson::Value().SetString(real_name.c_str(), real_name.size(), doc.GetAllocator()), i, doc.GetAllocator());
