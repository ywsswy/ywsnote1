## type a = b;
```
a =
    b; //缩进4个空格
```
## type func(type a) {
```
type func(
    type a {//缩进4个空格
```
## type func(type a, type b);
```
既有可能
type func(type a, type b);
又有可能
type func(type a,
          type b);
又有可能
type func(
    type a,
    type b);
首先考虑前者
```

## 非官方

“一种”格式化
//astyle -k3 --style=bsd <file>

clang-format <file> #默认会逐层往上找.clang-format配置文件，配置示例
```
Language: Cpp
BasedOnStyle: Google
ColumnLimit: 100

# 自动识别代码中的指针对齐方式，如果希望强制统一，则可以改为 false
DerivePointerAlignment: true

# 默认对齐到类型名
PointerAlignment: Left

# Only sort headers in each include block
SortIncludes: true
IncludeBlocks: Preserve
```
使用版本3以上的clang-format方法可以是：
```
yum install centos-release-scl -y
yum install llvm-toolset-7.0
scl enable llvm-toolset-7.0 bash #这句是单独开了一个bash，里面能用clang-foramt，exit之后就没了
```

## 换行中
写到尾部的：
&&
写到头部的：


## 官方
* 对于长的逻辑表达式，超过最大字符长度后，按以下格式缩进
```
if (this_one_thing > this_other_thing &&
    a_third_thing == a_fourth_thing &&  //4缩进
    yet_another &&
    last_one) {
  ... //2缩进
}
