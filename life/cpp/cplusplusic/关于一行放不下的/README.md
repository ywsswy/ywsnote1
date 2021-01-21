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


## 官方
* 指针/地址操作符 (*, &) 之后不能有空格
* 对于长的逻辑表达式，超过最大字符长度后，按以下格式缩进
```
if (this_one_thing > this_other_thing &&
    a_third_thing == a_fourth_thing &&
    yet_another &&
    last_one)
{
    ...
}