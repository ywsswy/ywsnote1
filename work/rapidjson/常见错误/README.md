## 1 只存地址，所以不能用临时变量，应该改成rapidjson::Value(2).Move()
conversion of argument 2 would be ill-formed:
main.cc:23:53: error: invalid initialization of non-const reference of type ‘rapidjson::GenericValue<rapidjson::UTF8<> >&’ from an rvalue of type ‘rapidjson::Value {aka rapidjson::GenericValue<rapidjson::UTF8<> >}’
     b2.AddMember("b", rapidjson::Value(2), allocator);
