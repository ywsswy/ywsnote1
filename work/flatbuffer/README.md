https://google.github.io/flatbuffers/flatbuffers_guide_building.html

cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release

make

make install

```
// Example IDL file for our monster's schema.
 
namespace MyGame.Sample;
 
enum Color:byte { Red = 0, Green, Blue = 2 }
 
union Equipment { Weapon } // Optionally add more tables.
 
struct Vec3 {
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


builder.Finish(orc);  // 注意

// 保存序列化数据
std::ofstream of1("of1", std::ios::binary);
of1.write(reinterpret_cast<const char*>(builder.GetBufferPointer()), builder.GetSize());



// 读取数据（读取前可以进行安全校验：https://stackoverflow.com/questions/37486992/flatbuffers-verifier-behaviour）
auto monster = GetMonster(s.c_str());
std::cout << "mana:" << monster->mana() << std::endl;
std::cout << "1:" << monster->weapons()->name()->str() << std::endl;
std::cout << "2:" << monster->weapons()->damage() << std::endl;
```

内置的例如string，使用builder.CreateString("xxx")，得到flatbuffers::Offset<flatbuffers::String>
自定义table，使用CreateXXX(builder, ...)，得到flatbuffers::Offset<XXX>
XXXBuilder xxx; xxx.Finish()，也能得到flatbuffers::Offset<XXX>
实际上，每次CreateXXXXX时，都会在builder中分配空间来写入数据


