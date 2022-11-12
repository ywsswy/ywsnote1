workspace 是目录树，有一个WORKSPACE文件的目录是workspace的根目录

workspace rules，用来写在



bzl文件里可以定义function，BUILD/WORKSPACE文件里禁止定义

导入其他文件中的function：
```
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
```

## 代码库规则
- http repository rules 通过http来创建仓库规则：下载某个代码库压缩包并解压
示例：
```
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

def my_workspace(path_prefix = "", tf_repo_name = "", **kwargs):
    fmtlib_ver = kwargs.get("fmtlib_ver", "7.1.3")
    fmtlib_name = "fmt-{ver}".format(ver = fmtlib_ver)
    http_archive(
        name = "com_github_fmtlib_fmt",
        strip_prefix = fmtlib_name,
        urls = [
            "https://github.com/fmtlib/fmt/archive/{ver}.tar.gz".format(ver = fmtlib_ver),
        ],
        build_file = str(Label("//:src/BUILD")),
    )
```
然后BUILD文件里写
```
cc_library(
    name = "fmtlib",
    hdrs = glob([
        "include/fmt/*.h",
    ]),
    defines = ["FMT_HEADER_ONLY"],
    includes = ["include"],
    visibility = ["//visibility:public"],
)
```
- git repository rules 通过http来创建仓库规则
示例
```
git_repository(
    name = "xxxx",
    remote = "https://github.com/xxxx/xxxx.git",
    branch = "master",
)
```