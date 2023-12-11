WORKSPACE文件，用于指定当前文件夹就是一个Bazel的工作区。所以WORKSPACE文件总是存在于项目的根目录下。
一个或多个BUILD文件，用于告诉Bazel怎么构建项目的不同部分。（如果工作区中的一个目录包含BUILD文件，那么它就是一个package。）
当Bazel编译项目时，所有的输入和依赖项都必须在同一个工作区。属于不同工作区的文件，除非linked否则彼此独立。

编译，//<查找BUILD文件的路径/packege路径>:<target>
bazel build //main:hello-world

查看依赖关系
bazel query --nohost_deps --noimplicit_deps 'deps(//main:hello-world)' --output graph
可以在http://www.webgraphviz.com/上绘制


一般来说一个target可能同时依赖：源文件、其他BUILD中的lib、本BUILD中的其他target，写法如下

cc_library( //这种是生成obj
    name = "hello-greet",
    srcs = ["hello-greet.cc"],
    hdrs = ["hello-greet.h"],
    linkopts = [""], //这里可以写类似"-pthread"的链接操作
)

cc_binary(  #这种是要生成可执行文件
    name = "hello-world",
    srcs = ["hello-world.cc"], # 自己的源文件
    deps = [
        ":hello-greet",      #找本BUILD中的其他target
        "//lib:hello-time",  #找本工作区的其他BUILD中的
    ],
)


## 一些示例

git@github.com:bazelbuild/examples.git

https://docs.bazel.build/versions/master/be/c-cpp.html#cc_library


## bazel clean --expunge #加了expunge，清理的更彻底？

## 单元测试，这种写法可以单步调试
bazel coverage ... -c dbg --coverage_report_generator="@bazel_tools//tools/test/CoverageOutputGenerator/java/com/google/devtools/coverageoutputgenerator:Main" --combined_report=lcov --nocache_test_results --strip=never --compilation_mode=dbg -s #查看输出日志可以查看二进制文件，用相对路径去执行
--test_output=all # 可以输出执行情况，或者直接去看bazel-out/k8-fastbuild/testlogs/<path_to_ut>/test.log


## 可以使用copts增加单元测试时的私有变量访问
## 推荐下面这种多包一层的写法，可以把_test.cc文件也显示覆盖率
```
cc_library(
    name = "test_lib",
    srcs = glob(
        ["*_test.cc", "mock_*.h"],
    ),  
    visibility = [ "//visibility:public",],  
    deps = [ 
        "@com_google_googletest//:gtest_main",
        ":xxx",
    ],  
    copts = [ 
      "-fno-access-control"
    ]   
)

cc_test(
    name = "test",
    visibility = ["//visibility:public",],  
    deps = [ 
        ":test_lib",
    ],
)
```

genhtml ./bazel-out/_coverage/_coverage_report.dat --output-directory result

## 输出日志在，
bazel-out/k8-fastbuild/testlogs/<path><test_bin>/test.log
## 反正bazel-out里面是最全的；bazel-out实际指向的是$HOME/.cache/bazel/_bazel_root/xxx里面的内容
每个单测的执行目录是
bazel-out/k8-dbg/bin/<path>/<test_bin>.runfiles/__main__

## 查看gcc的参数是在这个文件里：
bazel-out/k8-fastbuild/bin/<target_name>-2.params


# .bazelrc，命令行的参数优先级高于这个文件中的参数
这里面可以写一些默认编译参数，例如
build --cxxopt="--std=c++17"
build --copt=-O0
build --incompatible_no_support_tools_in_action_inputs=false
build --action_env=LIBRARY_PATH  # 使用外部环境变量，否则可能链接找不到系统库
build:RC --xxx=yyy  # 如果加了 ":RC" 表示配置分组，命令行bazel build时指定--config=RC这个分组的配置就会生效

# 可以加上 --remote_cache=http://<ip>:<port> 虽然还是本地编译，但是存储是在远端

## 一些编译参数
-c (dgb|opt) //分别表示-g 和 -O2 -DNDEBUG
--deleted_packages=<path to BUILD>
--sandbox_debug  # 显示verbose信息
--jobs 1
--subcommands  # 显示编译命令，类似verbose

# DONE
1）我的仓库依赖了A仓库和B仓库（写了git_repository rule），A仓库依赖了其他的库，我怎么知道A仓库依赖了哪些库，假设A仓库恰好也依赖了B仓库，这样我就可以省事不用亲自引入B仓库了
可以使用 bazel query --output=build //external:* |grep B 来看一下是否已经有人导入了B
2）怎么知道，谁的仓库里面添加（不叫依赖/使用，而是添加/创建）了C仓库，以便我看去看一下C版本
bazel query --output=build //external:C # --output=build表示 以 BUILD 中的格式输出目标形式
结果示例：可以得出结论：'根仓库的WORKSPACE:17' -> 'B仓库的path/workspace.bzl:369' 里面添加了C仓库
```
……
git_repository(
  name = "C",
  generator_name = "C",
  generator_function = "xxx",
  remote = "http://github.com/C/C.git",
  tag = "v0.1.2",  # 还支持branch/commit
)
# Rule C instantiated at (most recent call last):
#   path1/WORKSPACE:17:15                                            in <toplevel>
#   /root/.cache/bazel/_bazel_root/c6bece17efa9f8f3298e00447fcc75cb/external/B/path/workspace.bzl:369:19 in xxx
……
```
3）查看某target的直接依赖，直接看BUILD文件有时候不友好，例如BUILD文件里可能写的是srcs = glob(["src/*.cpp"]),但是这个命令可以把*展开
bazel query --output=build @repo_a//target_b

# Q
1）如果不知道A仓库里instantiated了B仓库，所以我还是手动写了B仓库的git_repository rule，那么最后的效果是redefine？还是只使用了其中一个（到底是哪一个？）？如果是B仓库的同版本或不同版本会带来不同的效果吗？
使用bazel query --output=build //external:C是不是也不一定显示的是真正使用的那个！是的(https://github.com/bazelbuild/bazel/issues/12947)！所以太坑了，继续找办法