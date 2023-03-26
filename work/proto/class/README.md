grpc::Status.ok() == false
grpc::Status.err_code() == 12 //这种是服务端pb有问题


google::protobuf::Map<>::iterator
google::protobuf::MapPair<>


google::protobuf::RepeatedPtrField的成员函数有https://developers.google.com/protocol-buffers/docs/reference/cpp/google.protobuf.repeated_field#RepeatedPtrField.RemoveLast.details，对RepeatedPtrField进行的复制是深拷贝
例如：begin()
end()
DeleteSubrange(start, num)
Clear()