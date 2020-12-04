grpc::Status.ok() == false
grpc::Status.err_code() == 12 //这种是服务端pb有问题



google::protobuf::Map<>::iterator
google::protobuf::MapPair<>



google::protobuf::RepeatedPtrField<Message_1> *p //这种是定义了repeated Message_1 name，通过mutable_name()得到的
p的成员函数有https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.repeated_field#RepeatedPtrField.RemoveLast.details
例如：p->begin()
end()
DeleteSubrange(start, num)
Clear()