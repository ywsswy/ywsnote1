template<typename T>
class TD;

int main()
{
    auto x = 3;
    TD<decltype(x)> test;
    return 0;
}



这种只声明，不定义，会诱发编译器报错，从而得到x的类型

main.cc: In function ‘int main()’:
main.cc:9:21: error: aggregate ‘TD<int> test’ has incomplete type and cannot be defined
     TD<decltype(x)> test;
                     ^
