# rpc
是一个框架，目标是让程序员调用远程服务像调本地函数一样简单。
需要多个组件组成这个效果：
- 接口描述（IDL）：定义服务端接口、参数；（可以用protobuf）
- 序列化：编码解码要网络传输的接口、参数；（可以用protobuf）
- stub/proxy（桩）：客户端直接调用的对象（易用性）；（可以用grpc or protobuf2）
- 网络通信：收发字节流，处理连接、并发；（可以用grpc or protobuf2+muduo）
- 协议封装：自定义协议头，解决 TCP 粘包；（可以用grpc or protobuf2+手搓）
- 服务注册发现（可以用zk）

# protobuf协议【A】
使用protoc编译器可以生成：
- 每个class的essage类【B】、对应序列化方法；
- proto2默认会生成 MathService_Stub 桩类（客户端用它）、google::protobuf::RpcChannel 抽象基类等，但是是空壳，RpcChannel::CallMethod 是纯虚函数，没有提供任何网络实现，需要开发者实现这种非常底层的细节；（很多人面试简历里的练手项目就是这样用的，在这里面实现服务发现、TCP连接序列化、收发字节流等逻辑）；
- 从proto3开始【已废弃】默认不再生成 service【C】 / Stub / RpcChannel（空壳）代码了，除非加特定 option；
- 从此，官方推荐的rpc变成了grpc：

# grpc
是protobuf 的 RPC 完整实现，由 Google 推出，是 protobuf 那套被废弃的原生 RPC 的"官方继任者"。
- 需要使用 protoc 插件：protoc-gen-grpc-cpp
- 重新设计了一套自己的 stub 和 channel 接口（grpc::Channel、grpc::ClientContext 等）
- 底层基于 HTTP/2 做传输（支持流式调用、多路复用、头部压缩），提供完整的网络实现（不再需要自己写）
- 不再需要自定义协议头，/服务名/方法名，直接放HTTP的URL里（优点：可以做“方法”粒度的限流）；所以可以直接使用curl http://<ip_port>/服务名/方法名 就能调试调用；
- 客户端使用【D】（Stub 构造时直接传 IP:Port（或者自定义Resolver解析器），网络层 gRPC 自己搞定）：

## 【A】
```
package <namespace_name>;
service <class_name> {
  rpc <function_name> (<Request_class_name>) returns (<Response_class_name>) {}
}
message <Request_class_name> {
  string data1 = 1;
}
message <Response_class_name> {
  string data2 = 1;
}
```

## 【C】
```
class <create_class_name> final : public <class_name>::Server {
  Status <function_name>(ServerContext *context, const <Request_class_name> *request, <Response_class_name> *reply) override {
    /* ... */ reply->set_data2(request->data1());/* ... */
    return Status::OK;
  }
};
```

## 【D】
```
std::unique_ptr<Greeter::Stub> stub_ = Greeter::NewStub(grpc::CreateChannel("<host>",grpc::InsecureChannelCredentials()));
stub_ -><function_name>(&context, request, &reply);
```