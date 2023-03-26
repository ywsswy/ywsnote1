#include<memory>


int main()
{
    std::shared_ptr<int> pointer = std::make_shared<int>(10);
    std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl; //引用计数/有多少个智能指针管理着这块内存
    std::shared_ptr<int> pointer2 = pointer;
    std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl; //2
    std::cout << "pointer2.use_count() = " << pointer2.use_count() << std::endl; //2
    std::cout << *(pointer2.get()) << std::endl; //get()后可以赋值给普通指针，get()本身并不会减少引用计数
    std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl;
    std::cout << "pointer2.use_count() = " << pointer2.use_count() << std::endl;
    pointer2.reset(new int(15));
    pointer2.reset();
    return 0;
}