# 字符串拼接的方式替换，可以用于pb字段的复制

#define XXXXX(a, b, key)                \
    do                                  \
    {                                   \
        if (a.has_##key()) {            \
            b->set_##key(a.key());      \
        }                               \
    } while (0)


# 这两种处理可变参数的宏效果是一致的

#include <cstdio>
#define AAA(...) printf("%d %d\n", __VA_ARGS__)
#define BBB(args...) printf("%d %d\n", ##args)

int main() {
        AAA(555, 555);
        BBB(444, 444);
        return 0;
}