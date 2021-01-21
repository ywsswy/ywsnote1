# 字符串拼接的方式替换，可以用于pb字段的复制

#define XXXXX(a, b, key)                \
    do                                  \
    {                                   \
        if (a.has_##key()) {            \
            b->set_##key(a.key());      \
        }                               \
    } while (0)
