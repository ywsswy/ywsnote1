https://bazel.build/extending/rules

使用的是[Starlark语言（python方言）](https://bazel.build/rules/language)

新增一个y.bzl文件
```
def yws_fun(
        name,
        srcs):
  print("\nhhh\n")
```

BUILD文件中即可使用
```
load("//:yws.bzl", "yws_fun")
yws_fun(
    name = "foo",
    srcs = ["foo.cc"],
)
```