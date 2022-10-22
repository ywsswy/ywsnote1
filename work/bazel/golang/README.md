golang可以用bazel来处理依赖
就是每个目录里面写一个BUILD.bazel
形如：
```
load("@io_bazel_rules_go//go:def.bzl", "go_library")

go_library(
    name = "index",
    srcs = [
        "create.go",
        "get.go",
    ],
    importpath = "<folder_name>",
    visibility = ["//visibility:public"],
    deps = [
        "//application/cli/container",
        "//application/cli/log",
    ],
)
```