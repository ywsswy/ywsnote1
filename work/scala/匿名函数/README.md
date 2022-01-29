```
// 有参数
// 1.1 标准函数的完整写法
def f1(x:String) : Int = { x.toInt }
// 省略函数体的花括号
def f2(x:String) : Int = x.toInt
// 省略返回值的写法
def f3(x:String) = x.toInt

// 1.2 匿名函数（省略返回值类型和函数名）的完整写法
// def/val 函数/变量 : 类型 = <匿名函数>
def f4 : String => Int = (x:String) => x.toInt
// 自动推导匿名函数返回值类型的写法
def f5 : String => Int = (x) => x.toInt
// 自动推导函数/变量返回值的写法
def f6 = (x:String) => x.toInt
// 单参数类型省略时，括号也可省略
def f51 : String => Int = x => x.toInt
// 单参数时，参数都可以省略
def f52 : String => Int = _.toInt


// 无参数
// 2.1 标准函数的完整写法
def f21() : Int = { 3 }
// 省略函数体的花括号
def f22() : Int = 3
// 省略返回值的写法
def f23() = 3

// 2.2 匿名函数（省略返回值类型和函数名）的完整写法
def f24 : () => Int = () => 3
// def f25 : () => Int = {3}  // 错误，因为{3}并不是匿名函数，没办法把{3}赋值给() => Int
// 自动推导返回值的写法
def f26 = () => 3
```