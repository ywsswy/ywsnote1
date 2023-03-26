std::string Dom2String(rapidjson::Document &d) 
{
     rapidjson::StringBuffer buffer;
     rapidjson::Writer<rapidjson::StringBuffer> writer(buffer);
     d.Accept(writer);
     return buffer.GetString();
}

std::string Value2String(rapidjson::Value &d) 
{
     rapidjson::StringBuffer buffer;
     rapidjson::Writer<rapidjson::StringBuffer> writer(buffer);
     d.Accept(writer);
     return buffer.GetString();
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