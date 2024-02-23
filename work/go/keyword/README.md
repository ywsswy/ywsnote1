nil 不是关键字，而是预定义的标识符代表了pointer, channel, func, interface, map 或者 slice 的zero value（https://zhuanlan.zhihu.com/p/151140497）
引用类型的变量，如果只声明未定义（用非nil值初始化）时，其值就是nil
var a *int  // 等价于var a *int = nil，不能直接使用，必须再用new初始化（a = new(int)，注意new语句只能分配空间，赋值需要再写一条语句）
var b int  // 等价于var b int = 0

## 基本类型
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // uint8 的别名

rune // int32 的别名
    // 表示一个 Unicode 码点

float32 float64

complex64 complex128 //复数

## 赋值
i := 1  // 首次出现变量i
j = 2  // 使用以前出现的j
// := 的操作符左侧不要求全部是新变量，只要存在新变量即可，例如如下三种情况均可；判断是哪一种情况的原则是，同作用域优先使用old，子作用域优先使用new
// new, new := xx()
// old, new := xx()
// new, old := xx()
var i, j int = 1, 2 //这个功能和上两句相同，但是前两句只能在写在函数里
i, j := 1, 2
const Pi = 3.14 //常量的赋值，跟变量的区别是可以存储高精度的值



// 多行字符串
s := `fwef
fawe
oo`

## 类型转换（不支持隐式转换，必须显示强转）
var z uint = uint(f)

for i := 0; i < 10; i++ {//循环的写法，不支持 ++i
	fmt.Println(i)
}

if {
} else if {
}


## 数组
	var a [2]string  // 数组长度固定，后续无法继续增加，如果动态的则使用切片
	var a2 [...]string{"fwe","ooo"}  // 还可以用...来确定数组长度，
	a[0] = "Hello"
	a[1] = "World" // 数组相比切片，在编译阶段就可以检查问题，例如这里写a[2] = "x"就会编译报错
	fmt.Println(a[0], a[1][3:]) // 字符串截取
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)

s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}


## 切片
var s []int = primes[1:4] // [1,4)区间
var s []int // 此时是nil，但是可以用 s = append(s, 6)来添加元素
b := make([]int, 0, 5) //动态 申请数组，0是len显式的元素个数，5是cap是该切片首地址往后允许写多少元素

## 函数指针
func compute(fn func(float64, float64) float64, a interface{}, b ...interface{}) float64 { //入参是函数指针
        aa := a.(int)  // 只能用写入的类型来解析，.(int32)或者.(int64)都会错误
        bb, ok := args[0].(int)  // ok true表示获取成功
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot), 6, 7, 8)  // interface{}可以接收任何类型的复制，...interface{}可以接收变长参数
