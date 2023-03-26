# protobuf
syntax = "proto3";

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

# server.cc

class <create_class_name> final : public <class_name>::Server {
  Status <function_name>(ServerContext *context, const <Request_class_name> *request, <Response_class_name> *reply) override {
    /* ... */ reply->set_data2(request->data1());/* ... */
    return Status::OK;
  }
};

# client.cc
std::unique_ptr<Greeter::Stub> stub_ = Greeter::NewStub(grpc::CreateChannel("<host>",grpc::InsecureChannelCredentials()));
stub_ -><function_name>(&context, request, &reply);


# 即 proto中rpc定义了客户端调用的函数名

https://www.envoyproxy.cn/v1apireference/httpfilters/grpcjsontranscoderfilter
add_whitespace = true 易读
always_print_primitive_fields = true 默认值不省略
preserve_proto_field_names = true 不变驼峰