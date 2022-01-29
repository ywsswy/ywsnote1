## keyworld/funtion?
### type
type \<new_type> = \<old_type>

### var/val <变量>
### def <函数>
```
def <function>(<参数列表>) [: return_type] = {
   function body
}
```
### for
for (i <- 0.until(3))  // 左闭右开的scala.collection.immutable.Range

## ps
- idea scala worksheet可以实时计算
右侧显示的格式是
<变量名>: <类型> [= <值>，ps：如果是函数则没有值]

```
example:
F0是一个函数，入参是
F0: ()Int

F1是一个值（使用var/val定义的），存的是一个匿名函数，该函数入参是空且返回值是Double
F1: () => Double = <function0>

F2是一个匿名函数（使用def定义的），该函数入参是空且返回值是Double
F2: () => Double
```

- 判断变量是否是某类型
.isInstanceOf[<type>]