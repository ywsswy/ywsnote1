GOROOT 就是go的安装目录，go和gofmt在里面

GOPATH
编译后二进制的存放目的地
import包时的搜索路径
pkg目录存放编译好的库文件, 主要是*.a文件
也可以是源代码地址

go mod init <此项目名> #创项目的时候执行该命令，可以生成一个go.mod文件，记载着。？

go run <src.go> #直接执行

go build #编译出二进制，然后再手动执行，如果有require的东西且本机还没有，会下载到$GOPATH/pkg/mod目录下

go install <domain/path> # 安装，在 $GOPATH/bin 下就会出现可执行文件

go mod tidy #  Go Mod会自动分析程序的依赖项并进行下载。因此在理想情况下可以方便的将旧有项目转换为Go Mod项目，会自动修改 go.mod 和生成具体的 go.sum 。
前者是版本依赖的 语义化描述 ，后者则是实际所下载的代码版本信息(如commit id或具体的某个tag)。

go get命令中包含了go build和go install，实际上有两个前置步骤：①执行一次HTTP GET请求获取元数据(缺省是HTTPS, 也支持SSH); ②根据返回的元数据中的地址获取代码，一般就是git clone
使用HTTPS的话需要验证对方站点是否可信，此时要用到CA证书；使用SSH的话实际上是通过将生成的公钥存放到代码托管网站而自己持有私钥，这样就能加密并验证对方是否可信
Go Mod 在下载依赖项的时候，本质上就是使用 go get 进行下载 //会下载到$GOPATH/pkg/mod里面

go get xxx@master # 缺少依赖的时候可以这样下载，会自动修改go.mod

基本类型
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
i := 1
j := 2
var i, j int = 1, 2 //这个功能和上两句相同，但是前两句只能在写在函数里
i, j := 1, 2
const Pi = 3.14 //常量的赋值，跟变量的区别是可以存储高精度的值

## 类型转换（不支持隐式转换，必须显示强转）
var z uint = uint(f)

for i := 0; i < 10; i++ {//循环的写法，不支持 ++i
	fmt.Println(i)
}

## 结构体
type Vertex struct {
	X, Y int
}

func main() {
	var a = 3
	b := &a
	var c = Vertex{4,5}
	d := &c
	fmt.Println(a, b, c, d, d.X, (*d).X) //后两种写法等价

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
## 数组
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
## 切片（是引用）
var s []int = primes[1:4] // [1,4)区间
var s []int // s nil, len(s) 0显式的元素个数, cap(s) 0像是该切片首地址往后允许写多少元素
b := make([]int, 0, 5) //动态 申请数组，0是len，5是cap

## map
var m map[string]Vertex // c++的map<string, Vertex>



func compute(fn func(float64, float64) float64) float64 { //入参是函数指针
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))

	fmt.Println(compute(hypot))


## 结构体/类的方法
```
type Vertex struct {
	X float64
}


func (this *Vertex) Double() { //就是在普通函数的函数名前面加上(this *<class>)，注意必须是指针，不然是深拷贝
	this.X = this.X * 2
}

func main() {
	v := Vertex{3}
	v.Double() // (&v).Double() 也能调用成功，go对指针能判断该用指针本身还是用指针指向的对象
	fmt.Println(v)
}
```

## example
```
package main

import "fmt"

const Pi = 3.14

func main() {
	const World = "世界"
	fmt.Println("Hello", World)//这种输出是空格连接的
}

```