## 非官方

“一种”格式化
astyle -k3 --style=bsd <file>




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