// 1.有协议+json时
  flatbuffers::IDLOptions opts;
  opts.skip_unexpected_fields_in_json = true;
  flatbuffers::Parser parser(opts);
  if (parser.Parse(gSchema.c_str(), nullptr) != true) {
    std::cout << "ERROR1" << std::endl;
    return;
  }
  if (parser.Parse(gJson.c_str()) != true) {  // 既可以用Parse还可以用ParseJson
    std::cout << "ERROR2:" << parser.error_ << std::endl;
    return;
  }
  std::string s2(reinterpret_cast<char*>(parser.builder_.GetBufferPointer()), parser.builder_.GetSize());

// 2.只有桩代码时，还在调研










#include <iostream>
#include "flatbuffers/idl.h"
#include "flatbuffers/minireflect.h"
#include "item_embedding_generated.h"

std::string BufferToJsonString(const uint8_t* buffer, const flatbuffers::TypeTable* type_table) {
  flatbuffers::ToStringVisitor tostring_visitor("", true, "", true);
  IterateFlatBuffer(buffer, type_table, &tostring_visitor);
  return tostring_visitor.s;
}

std::string gSchema = R"(
//flatbuffers Embedding struct
namespace PrimaryEngine.Flatbuffers;
enum Color:byte { Red = 0, Green, Blue = 2 }
struct Vec3  {
    x:float;
    y:float;
    z:float;
}
table Embedding {
    pos:Vec3;
    mana:short = 150;
    hp:short = 100;
    name:string;
    inventory:[ubyte];
    color:Color = Blue;
}
root_type Embedding;
)";

std::string gJson = " {\"mana\": 123, \"hp\":555, \"xxxxxx\":777}";

namespace PrimaryEngine {
namespace Flatbuffers {

void f2() {
  flatbuffers::IDLOptions opts;
  opts.skip_unexpected_fields_in_json = true;
  flatbuffers::Parser parser(opts);
  if (parser.Parse(gSchema.c_str(), nullptr) != true) {
    std::cout << "ERROR1" << std::endl;
    return;
  }
  if (parser.ParseJson(gJson.c_str()) != true) {  // 既可以用Parse还可以用ParseJson
    std::cout << "ERROR2:" << parser.error_ << std::endl;
    return;
  }
  std::string s =
      BufferToJsonString(parser.builder_.GetBufferPointer(), Embedding::MiniReflectTypeTable());
  std::cout << s << std::endl;
}
}  // namespace Flatbuffers
}  // namespace PrimaryEngine

int main() { PrimaryEngine::Flatbuffers::f2(); }
