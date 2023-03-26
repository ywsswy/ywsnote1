Value a(123);
Value b(456);
b = a;         // a 变成 Null，b 变成数字 123。
#这种设计是为了大型数据操作时避免耗时地深拷贝，和省去临时对象销毁的开销

因此 对赋值采用转移语义。这方法与 std::auto_ptr 相似。
除了赋值，还有AddMember(), PushBack()也是转移语义



# 以下无视
# 深拷贝 浅拷贝
- 深拷贝是一个Move的语义，反正之后二者只有一个才能获取到？
1）
void function(&a)
{
	rapidjson::Document::AllocatorType &allocator = a.GetAllocator();
	rapidjson::Document b1;
	rapidjson::Document b2;
        rapidjson::Value b3;
	a.AddMember("test1", b1, allocator);//这种方式b2是以浅拷贝的形式放到a中，执行完这个函数后，b2析构掉，a就找不到test1了
	a.AddMember("test2", rapidjson::Value(b2, allocator), allocator);//这种方式才是深拷贝
	a.AddMember("test3", b3, allocator);//这种方式也不怕析构（应该是Value会听话，Document不指定就不听话）
}
2）GetObject是深拷贝

原document可以找到的东西
如果经过GetObject给了谁，
那么document可能就找不到那个东西了