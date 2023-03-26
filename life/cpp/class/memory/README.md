#include <memory>
#include <iostream>
int main() {
    auto pointer = std::make_shared<int>(10);
    // 等价于std::shared_ptr<int> pointer(new int(10));  // 前者是传初始化参数进行一次int的构造，可以灵活写成复制构造或者std::move(10)移动构造等方式，后者是传指针直接接管内存而没有发生int的构造
    std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl;  // 1引用计数/有多少个智能指针管理着这块内存
    std::shared_ptr<int> pointer2 = pointer;
    std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl;  // 2
    std::cout << "pointer2.use_count() = " << pointer2.use_count() << std::endl;  // 2
    std::cout << *(pointer2.get()) << std::endl; //get()后可以赋值给普通指针，get()本身并不会减少引用计数
    std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl;  // 2
    std::cout << "pointer2.use_count() = " << pointer2.use_count() << std::endl;  // 2
    pointer2.reset(new int(15));
    std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl;  // 1
    std::cout << "pointer2.use_count() = " << pointer2.use_count() << std::endl;  // 1
    pointer2.reset();
    std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl;  // 1
    std::cout << "pointer2.use_count() = " << pointer2.use_count() << std::endl;  // 0
    return 0;
}