https://flatbuffers.dev/flatbuffers_guide_use_cpp.html

# 生成flatc
cmake -G "Unix Makefiles"
```
// Example IDL file for our monster's schema.
 
namespace MyGame.Sample;
 
enum Color:byte { Red = 0, Green, Blue = 2 }
 
union Equipment { Weapon } // Optionally add more tables.
 
struct Vec3 {  // 跟table的区别是，struct是紧凑的每个字段都不可设置为optional的；
  x:float;
  y:float;
  z:float;
}

table Monster {
  pos:Vec3; // Struct.
  mana:short = 150;
  hp:short = 100;
  name:string;
  friendly:bool = false (deprecated);
  inventory:[ubyte];  // Vector of scalars.
  color:Color = Blue; // Enum.
  weapons:[Weapon];   // Vector of tables.
  equipped:Equipment; // Union.
  path:[Vec3];        // Vector of structs.
}
 
table Weapon {
  name:string;
  damage:short;
}
 
root_type Monster;
```

flatc --cpp monster.fbs

```
// g++ main.cc -std=c++11

#include "monster_generated.h"
using namespace MyGame::Sample;


flatbuffers::FlatBufferBuilder builder(1024);

// 创建内置类型：字符串
flatbuffers::Offset<flatbuffers::String> weapon_one_name = builder.CreateString("Orc");
// 创建一个sub-table的时候，需要一次性传递所有的成员（有没有更好的方法？）
flatbuffers::Offset<MyGame::Sample::Weapon> sword = CreateWeapon(builder, weapon_one_name, 123);
// 创建root_type的方法1，就跟创建sub-table一样
flatbuffers::Offset<MyGame::Sample::Monster> orc = CreateMonster(builder, 123, name, axe);
// 创建root_type的方法2，这种方法可以一个成员一个成员的设置，比较友好，最后需要使用.Finish
MonsterBuilder monster_builder(builder);
monster_builder.add_name(weapon_one_name);  // 注意不能.add_name(builder.CreateString("Orc")) ，必须提前分配好，就是说builder不能同时分配两个，必须一个已经Finish了，才能create下一个
monster_builder.add_mana(123);  // 注意每个成员只能add一次，否则会assert出错
flatbuffers::Offset<MyGame::Sample::Monster> orc = monster_builder.Finish();

builder.Finish(orc);  // 如果需要对orc进一步加工包装可以继续调用builder.StartTable(),然后进行AddOffset将 flatbuffers::Offset<void>(orc.o)继续放到一个table里

// 上面是有桩代码的情况下的构建buffer（使用builder.GetBufferPointer()拿到buffer）
// 如果没有桩代码，知道每个字段的offset和类型，也是能构建出一个table的buffer的，前提是这个table的每个字段必须是标量、或者string、或者标量数组，代码如下：
flatbuffers::FlatBufferBuilder builder(1024);
// 1. 首先把[byte]和string这种分配出来，记录一下offset，因为这个不是不同的标量，没法跟普通标量放在同一个连续空间
std::map<flatbuffers::voffset_t, flatbuffers::uoffset_t> offset_map;
// 1.2.1 pod数组的写法
builder.StartVector(buff_len / sizeof(T), sizeof(T));
builder.PushBytes(reinterpret_cast<const uint8_t *>(buff_data), buff_len);
uoffset_t uoffset = builder->builder.EndVector(buff_len / sizeof(T));
offset_map.insert({offset, uoffset});  // 这个offset是通过该字段的(下标_0s+2)*2算得
// 1.2.2 对象数组的写法
std::vector<flatbuffers::Offset<void>> ab_tables;
ab_tables.push_back(flatbuffers::Offset<void>(xxx_offset));
ab_tables.push_back(flatbuffers::Offset<void>(yyy_offset));
auto ab_tables_offset = builder.CreateVector<flatbuffers::Offset<void>>(ab_tables);
offset_map.insert({offset, uoffset});
// 1.3 string的写法
Offset<String> uoffset = builder->builder.CreateString(buff_data, buff_len).o;
offset_map.insert({offset, uoffset});  // 这个offset是通过该字段的(下标_0s+2)*2算得
// 2. 然后开始构建table（放标量）
uoffset_t start_offset = builder.StartTable();
// 2.1 普通标量的写法
builder.AddElement<T>(offset, 666, 0);  // offset填该字段vtable中的voffset偏移
// 2.2 bool的写法
builder.AddElement<uint8_t>(offset, true, 0);
// 3. （放之前的offset）
for (auto&& it : offset_map) {     
  builder.AddOffset(it.first, flatbuffers::Offset<void>(it.second));
}
// 4. 结束，然后builder.GetBufferPointer() 就可以拿到buffer
builder.Finish(flatbuffers::Offset<void>(builder.EndTable(start_offset)));



// 保存序列化数据
std::ofstream of1("of1", std::ios::binary);
of1.write(reinterpret_cast<const char*>(builder.GetBufferPointer()), builder.GetSize());


// （有桩代码时）读取数据（读取前可以进行安全校验：https://stackoverflow.com/questions/37486992/flatbuffers-verifier-behaviour）
auto&& monster = GetMonster(s.c_str());  // 从序列化数据获取，仅这一步需要操作序列化数据，之后即使是处理[ubyte] (nested_flatbuffer: "XXX")字段也不会用GetXXXX(序列化数据)的操作；
std::cout << "mana:" << monster->mana() << std::endl;
if (monster->weapons()->name() != nullptr) {  // 字符串必须要保护
  std::cout << "1:" << monster->weapons()->name()->str() << std::endl;
}
std::cout << "2:" << monster->weapons()->damage() << std::endl;
for (auto vi1 : monster->vec1) {  // 数组
  vi1->yyy();
  auto zzz_root = flatbuffers::GetRoot<XXX>(vi1->zzz()->Data()); // zzz类型是[ubyte] (nested_flatbuffer: "XXX")
  zzz_root->www();
}


// => json_str （有桩代码时）
#include "flatbuffers/minireflect.h"
std::string BufferToJsonString(const uint8_t* buffer, const flatbuffers::TypeTable* type_table) {
  flatbuffers::ToStringVisitor tostring_visitor("", true, "", true);
  IterateFlatBuffer(buffer, type_table, &tostring_visitor);
  return tostring_visitor.s;
}
std::string json_str = BufferToJsonString(buffer, Monster::MiniReflectTypeTable());

// => json_str （有协议schema时）
std::string json_str;
if (!GenerateText(parser, buffer, &json_str)) {  // parser的类型是flatbuffers::Parser
  std::cout << parser->error_ << std::endl;
}

// 通过反射获取到一个table中定义了哪些成员
      for (size_t i = 0; i < Monster::MiniReflectTypeTable()->num_elems; i++) {
        new_fields.push_back(Monster::MiniReflectTypeTable()->names[i]);
      }
```

内置的例如string，使用builder.CreateString("xxx")，得到flatbuffers::Offset<flatbuffers::String>
自定义table，使用CreateXXX(builder, ...)，得到flatbuffers::Offset<XXX>
XXXBuilder xxx; xxx.Finish()，也能得到flatbuffers::Offset<XXX>
实际上，每次CreateXXXXX时，都会在builder中分配空间来写入数据

FlexBuffers 是 FlatBuffers 中一个 schema-less 的版本

// Q: generated.h -> parser  json_str->buffer