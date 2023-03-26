如果是可选编译，例如hh.h的方法有两种实现，分别在h1.cc和h2.cc，可以在编译阶段指定用h1.cc或者h2.cc
```
load("@bazel_skylib//rules:common_settings.bzl", "string_flag")  # 引入参考 https://github.com/bazelbuild/bazel-skylib

string_flag(  # 创建一个xxx参数，并指定默认值
    name = "xxx",
    build_setting_default = "h1",
)

config_setting(  # 当xxx参数是h1是，这个条件生效
    name = "h1_build",
    flag_values = {"xxx": "h1"},
)

config_setting(  # 当xxx参数是h2是，这个条件生效
    name = "h2_build",
    flag_values = {"xxx": "h2"},
)

cc_library(
    name = "lib",
    srcs = glob(
        ["main.cc"],
    ) + select({  # 根据条件选择，然后拼接起来
        ":h1_build": glob([  # 如果这个条件生效，则用这部分
            "h1.cc",
        ]),
        ":h2_build": glob([
            "h2.cc",
        ]),
    }),
    hdrs = glob(
        ["hh.h"],
    ),
)
```

命令行指定方式：
bazel build :bin --//:xxx=h2

.bazelrc中指定方式
build --//:xxx=h2