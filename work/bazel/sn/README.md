安装：
wget https://github.com/bazelbuild/bazel/releases/download/5.3.2/bazel-5.3.2-installer-linux-x86_64.sh && ./bazel-5.3.2-installer-linux-x86_64.sh

默认安装的版本有内置的Embedded JDK，首次运行bazel时会自动解压到~/.cache/bazel/_bazel_<user>/install目录里；
【踩坑】如果要跑单元测试覆盖率，请必须安装自带有内置的Embedded JDK的bazel版本 & 并且千万不要使用--spawn_strategy=local参数，否则覆盖率不是沙箱运行，都共享一个执行目录可能会导致覆盖率收集不全；

查看内置的java版本
bazel info java-home，然后去里面看


workspace 是目录树，有一个WORKSPACE文件的目录是workspace的根目录

workspace rules，用来写在bzl文件里可以定义function，BUILD/WORKSPACE文件里禁止定义

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
- git repository rules 通过git来创建仓库规则
示例
```
git_repository(
    name = "xxxx",
    remote = "https://github.com/xxxx/xxxx.git",
    branch = "master",
)
```


### 自定义规则
见write_rule