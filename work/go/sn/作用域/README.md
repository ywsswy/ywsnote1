var (
  funcCCTVMain = "cctv_main_caller" //该文件？中的全局
)

func fun() {  // 函数名首字母小写，则属于非导出函数，只能在该包内显式调用
  funcCCTVMain = "hello1" //全局
  fmt.Printf("%s\n", funcCCTVMain) //全局hello1
  funcCCTVMain := "hello2" //因为用了:=，是新定义了一个局部变量，并不像python类型是动态的
  fmt.Printf("%s\n", funcCCTVMain) //局部hello2
}


func main() {
  fmt.Printf("%s\n", funcCCTVMain) //cctv_main_caller
  fun()
  fmt.Printf("%s\n", funcCCTVMain) //hello1
}

## 生命周期不等于作用域
(编译器判断变量的生命期会超出作用域后，会自动在堆上分配)[https://go.dev/doc/faq#stack_or_heap]
如下例子中看似b是局部变量，但是其实编译器发现有取地址操作，所以是在堆上分配的，b的生命周期就延长了
```
func f(a *int) {
	b := 9
	a = &b
}
```

## 多个变量 := 的情况下作用域见：https://ywsswy.top/ywsnote1/?path=/home/hill/workspace/github.com/ywsswy/ywsnote1/work/go/keyword
