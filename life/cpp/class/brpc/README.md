AAAService_Stub对应的就是调用service AAAService的proxy

brpc::Channel channel;
channel.Init(ip_port);

AAAService_Stub stub(&channel);

brpc::Controller cntl;
cntl.set_timeout_ms(xxxx);

stub.get(&cntl, &request, &response, nullptr);  // 返回值是void


// 超时要从cntl来判断

 if (cntl.Failed()) {
  if (cntl.ErrorCode() == brpc::ERPCTIMEDOUT) {
    // timeout
  }
 }




a.proto生成的是a.pb.h
service AService {rpc f(FRequest) returns (FResponse);
对应就是
class AService : public ::PROTOBUF_NAMESPACE_ID::Service {
 public:
    virtual void f(::PROTOBUF_NAMESPACE_ID::RpcController* controller, const ::FRequest* request, ::FResponse* response,
                   ::google::protobuf::Closure* done);

用户可以再继承，然后实现f函数
class AServiceImpl : public AService, public butil::SimpleThread {
 public:
  void f(::google::protobuf::RpcController* controller, const FRequest* request, FResponse* response,
         ::google::protobuf::Closure* done) {
    //...
    done->Run(); 这时才回包
  }