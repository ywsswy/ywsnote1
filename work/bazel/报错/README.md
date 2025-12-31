如果一个镜像可以另一个镜像不可以，
报错不是因为头文件找不到，那就肯定是因为bazel版本不同导致语法不同；

如果感觉linkopts和copts不生效，可能是存在循环依赖/重复编译，例如a写了srcs = glob(["*.cpp"])，b写了 srcs = glob(["b.cpp"])，然后b又依赖a；


- is being linked both statically and dynamically into this executable
可能是同一个源文件被引入的多次，
cc_test里写了srcs = ["a.cpp"],
cc_library里也写了srcs = ["a.cpp"],
然后cc_test还依赖这个cc_library；