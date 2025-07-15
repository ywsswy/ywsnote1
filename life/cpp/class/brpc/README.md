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