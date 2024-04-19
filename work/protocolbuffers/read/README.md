message A
{
  string b;
};

// pb.h

class A
{
::PROTOBUF_NAMESPACE_ID::internal::ArenaStringPtr b_; //string的pb内部类
}


// 注：
::PROTOBUF_NAMESPACE_ID等价于google::protobuf

core_dump问题，google::protobuf::internal::ArenaStringPtr::Set
set_b(const char*)
messageToJsonstring等
基本都是因为pb文件不对，多处重定义，为重新生成等问题

