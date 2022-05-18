#include "rapidjson/include/rapidjson/writer.h"
#include "rapidjson/include/rapidjson/prettywriter.h"

std::string DocumentToString(rapidjson::Document& d, const int indent = 0) { 
  rapidjson::StringBuffer buffer;
  if (indent == 0) {
    rapidjson::Writer<rapidjson::StringBuffer> writer(buffer);
    d.Accept(writer);
    return buffer.GetString();
  } else {
    rapidjson::PrettyWriter<rapidjson::StringBuffer> writer(buffer);
    writer.SetIndent(' ', indent);
    d.Accept(writer);
    return buffer.GetString();
  }
}

std::string ValueToString(rapidjson::Value &d, const int indent = 0) {
  rapidjson::StringBuffer buffer;
  if (indent == 0) {
    rapidjson::Writer<rapidjson::StringBuffer> writer(buffer);
    d.Accept(writer);
    return buffer.GetString();
  } else {
    rapidjson::PrettyWriter<rapidjson::StringBuffer> writer(buffer);
    writer.SetIndent(' ', indent);
    d.Accept(writer);
    return buffer.GetString();
  }
}

// 不要手动拼json，当涉及到一个object序列化成字符串作为另一个object中的value，需要一层层考虑转义字符
{
  "a": ""
}
{
  "b": "{\"a\":\"\"}"
}
{
  "c": "{\"b\":\"{\\\"a\\\":\\\"\\\"}\"}"
}