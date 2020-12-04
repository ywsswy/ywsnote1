#include <iostream>
#include <fstream>
#include "yws.pb.h"
#include <google/protobuf/util/json_util.h>


std::string ShowReal(Yws &a)
{
    std::string s;
    ::google::protobuf::util::JsonPrintOptions options; //default hump
    options.always_print_primitive_fields = true;//also print default value
    options.preserve_proto_field_names = true;
    ::google::protobuf::util::MessageToJsonString(a, &s, options);
    return s;
}

int main()
{
    Yws a;
    a.add_testrepeat(1);
    a.add_testrepeat(2);
    a.add_testrepeat(3);
    std::cout << ShowReal(a) << std::endl;
    //a.mutable_testrepeat() 相当于拿到一个map的指针
    for (google::protobuf::RepeatedField<int>::iterator iter = a.mutable_testrepeat()->begin(); iter != a.mutable_testrepeat()->end();)
    {
        if (*iter == 2)
        {
            iter = a.mutable_testrepeat()->erase(iter);
        }
        else
        {
            ++iter;
        }
    }
    std::cout << ShowReal(a) << std::endl;
    return 0;
}

