WORKSPACE文件，用于指定当前文件夹就是一个Bazel的工作区。所以WORKSPACE文件总是存在于项目的根目录下。
一个或多个BUILD文件，用于告诉Bazel怎么构建项目的不同部分。（如果工作区中的一个目录包含BUILD文件，那么它就是一个package。）
当Bazel编译项目时，所有的输入和依赖项都必须在同一个工作区。属于不同工作区的文件，除非linked否则彼此独立。

编译，//<查找BUILD文件的路径/packege路径>:<target>
bazel build //main:hello-world


查看依赖关系
bazel query --nohost_deps --noimplicit_deps 'deps(//main:hello-world)' --output graph


一般来说一个target可能同时依赖：源文件、其他BUILD中的lib、本BUILD中的其他target，写法如下

cc_library( //这种是生成obj
    name = "hello-greet",
    srcs = ["hello-greet.cc"],
    hdrs = ["hello-greet.h"],
    linkopts = [""], //这里可以写类似"-pthread"的链接操作
)

cc_binary(  //这种是要生成可执行文件
    name = "hello-world",
    srcs = ["hello-world.cc"], // 自己的源文件
    deps = [
        ":hello-greet",      //找本BUILD中的其他target
        "//lib:hello-time",  //找本工作区的其他BUILD中的
    ],
)


## 一些示例

git@github.com:bazelbuild/examples.git

https://docs.bazel.build/versions/master/be/c-cpp.html#cc_library


## bazel clean --expunge #加了expunge，清理的更彻底？

## 单元测试，这种写法可以单步调试
bazel coverage ... -c dbg --coverage_report_generator="@bazel_tools//tools/test/CoverageOutputGenerator/java/com/google/devtools/coverageoutputgenerator:Main" --combined_report=lcov --nocache_test_results --strip=never --compilation_mode=dbg -s #查看输出日志可以查看二进制文件，用相对路径去执行
--test_output=all # 可以输出执行情况

## 可以使用copts增加单元测试时的私有变量访问
cc_test(
    name = "test",
    srcs = glob(
        ["*_test.cc", "mock_*.h"]
    ),
...
    copts = [
      "-fno-access-control"
    ]
)


genhtml ./bazel-out/_coverage/_coverage_report.dat --output-directory result

## 输出日志在，
bazel-out/k8-fastbuild/testlogs/<path><test_bin>/test.log
## 反正bazel-out里面是最全的
每个单测的执行目录是
bazel-out/k8-dbg/bin/<path>/<test_bin>.runfiles/__main__

# .bazelrc
这里面可以写一些默认编译参数，例如
build --cxxopt="--std=c++17"
build --copt=-O0
build --incompatible_no_support_tools_in_action_inputs=false

## 一些编译参数
-c (dgb|opt) //分别表示-g 和 -O2 -DNDEBUG
--deleted_packages=<path to BUILD>
--sandbox_debug
--jobs 1