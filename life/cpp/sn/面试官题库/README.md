🟢 简单级别
1. C++ 中 struct 和 class 的区别？
答： 默认访问权限不同：struct 默认 public，class 默认 private；默认继承方式也一样。其余功能完全相同。

2. const 和 #define 的区别？
答：
const 有类型检查，编译器处理，存储在符号表或内存中；

#define 是预处理器文本替换，无类型检查，无作用域限制；

调试时 const 可见，#define 不可见。

4. new/delete 和 malloc/free 的区别？
答：

new/delete 是运算符，会调用构造/析构函数；

malloc/free 是 C 库函数，只分配/释放内存；

new 失败抛 std::bad_alloc，malloc 失败返回 nullptr；

new 不需要手动计算大小和类型转换。

5. 什么是内联函数（inline）？有什么优缺点？
答：

编译器将函数调用处展开为函数体，减少函数调用开销；

优点：消除调用开销，适合短小频繁调用的函数；

缺点：代码膨胀，增大二进制体积，inline 只是建议，编译器可忽略。

6. nullptr、NULL、0 的区别？
答：

0 是整型字面量；

NULL 通常是 #define NULL 0，类型不明确；

nullptr 是 C++11 引入的空指针字面量，类型为 std::nullptr_t，可以隐式转换为任意指针类型，重载时不会产生歧义。

7. 什么是 RAII？
答： Resource Acquisition Is Initialization，资源获取即初始化。将资源的生命周期绑定到对象的生命周期，构造时获取资源，析构时释放资源。典型例子：std::lock_guard、std::unique_ptr、文件句柄封装类。

🟡 中等级别
1. 虚函数的实现原理（vtable）？
答：

每个含虚函数的类有一张虚函数表（vtable），存储虚函数指针；

每个对象头部有一个虚指针（vptr），指向所属类的 vtable；

调用虚函数时，通过 vptr -> vtable -> 函数指针 实现动态分派；

虚函数调用有一次间接寻址开销，无法内联。

4. std::shared_ptr 的实现原理及循环引用问题？
答：

对象会在堆上创建；

内部维护引用计数（强引用 + 弱引用），线程安全地原子操作；

强引用计数为 0 时释放对象，弱引用计数为 0 时释放控制块；

循环引用：A和B的成员变量都有智能指针，并且A的智能指针指向 B，B的智能指针指向 A；

解决：将其中一方改为 std::weak_ptr（持有对对象的弱引用，也就是不会增加对象的引用计数，构造还是用 = std::make_shared()来构造，使用前需要调用.lock()获取一个指针指针）；


5. volatile 关键字的作用？
答：

告诉编译器该变量可能被外部（硬件、其他线程、信号）修改，禁止编译器对其进行优化（如缓存到寄存器、重排序）；

每次访问都从内存读取；

注意：volatile 不保证原子性，不能替代互斥锁或 std::atomic。

7. std::vector 扩容机制？
答：

容量不足时，通常按 1.5x（MSVC） 或 2x 扩容；

分配新内存 → 移动/拷贝旧元素 → 释放旧内存；

push_back 均摊时间复杂度 O(1)；

可用 reserve() 预分配避免频繁扩容；shrink_to_fit() 释放多余容量。

8. 什么是内存对齐？为什么需要？
答：

CPU 访问内存时，按对齐地址访问效率最高，否则需要多次总线操作甚至硬件异常；

结构体成员按自身大小对齐，整体大小是最大成员对齐数的倍数；

可用 alignas、#pragma pack 控制对齐；

合理排列成员顺序可减少 padding，节省内存。

🔴 困难级别
1. C++ 内存模型与 std::atomic 的内存序？
答：

C++11 定义了多线程内存模型，核心是happens-before关系；

std::atomic 提供六种内存序：

memory_order_relaxed：只保证原子性，无同步；

memory_order_acquire：读操作，后续操作不能重排到此前；

memory_order_release：写操作，之前操作不能重排到此后；

memory_order_acq_rel：读改写，兼具 acquire + release；

memory_order_seq_cst：全序一致，最强，默认值；

memory_order_consume：数据依赖序（实践中通常提升为 acquire）；

合理选择内存序可在保证正确性的前提下减少内存屏障开销。

2. 虚继承的原理及解决的问题？
答：

菱形继承问题：D 继承 B 和 C，B/C 都继承 A，导致 D 中有两份 A 的数据；

虚继承（virtual public A）使 A 的子对象只存在一份，通过虚基类指针（vbptr）和虚基类表（vbtable）定位；

代价：增加间接寻址开销，构造顺序复杂（最派生类负责构造虚基类）；

实际工程中应尽量避免菱形继承。

4. 完美转发（Perfect Forwarding）的原理？
答：

利用万能引用（T&& 在模板推导中）和引用折叠规则：

T& & → T&，T& && → T&，T&& & → T&，T&& && → T&&

std::forward<T>(arg) 在 T 为左值引用时转发为左值，为右值引用时转发为右值；

典型用途：包装函数、emplace 系列接口；

template<typename T, typename... Args>
std::unique_ptr<T> make_unique(Args&&... args) {
    return std::unique_ptr<T>(new T(std::forward<Args>(args)...));
} 
5. std::unordered_map 的实现原理及性能分析？
答：

基于哈希表，拉链法（开链法）解决冲突；

关键参数：load_factor = size / bucket_count，超过阈值（默认 1.0）触发 rehash；

rehash 代价高（O(n)），可用 reserve() 预分配桶数；

最坏情况（哈希碰撞严重）退化为 O(n)，需自定义高质量哈希函数；

与 std::map（红黑树，O(log n)）对比：平均 O(1) 但常数大，内存不连续，缓存不友好。


7. 如何排查 C++ 程序的内存泄漏？
答：

工具层面：

Valgrind --leak-check=full：检测泄漏、越界、use-after-free；

AddressSanitizer (ASan)：编译时插桩，-fsanitize=address，性能损耗小；

gperftools/tcmalloc 的 heap profiler；

代码层面：

优先使用智能指针（unique_ptr/shared_ptr）；
遵循 RAII，避免裸 new；